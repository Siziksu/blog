+++
type = "post"
title = "Kotlin Broadcast Channel in Android"
summary = "This post will explain how to use Kotlin Broadcast Channels in Android."
date = 2019-07-14T20:25:18+02:00
categories = ["kotlin", "android"]
tags = ["broadcast", "channel", "kotlin", "android"]
image = "images/back-view-boats-couple-2612852.jpg"
edit = ""
draft = false
+++
Ok, so you are the kind of programmer that uses Rx not only for background tasks, but also to subscribe to events and update things via subscription to subjects.
But, now you are using coroutines and want to mimic that so... how can you do that with coroutines?

Well, the straight answer is `Broadcast Channels`.

> A `Broadcast Channel` is a non-blocking primitive for communication between a sender and multiple receivers that subscribe for the elements.

In this example we are going to do a simple application with a `Broadcast Channel`, a `Sender` and two `Receivers`. For this I will try to build something as `real` as possible to end up being something more usefull that the usual stuff that you read that, when you try to implement, never works as you expected. So lets get started.

Also, in this example I will be using [Koin](https://github.com/InsertKoinIO/koin) as an Injector.

> You can find the source code here: [KotlinBroadcastChannel](http://github.com/Siziksu/kotlin-broadcast-channel).

<br />

## The channel

First of all, we need a channel to publish in and to observe from. What we want here, is a single channel where we will be sending data to, and multiple receivers (observers) that will subscribe to and get the data from. This kind of channel is called `Broadcast Channel`.

{{< highlight Kotlin >}}
val channel: BroadcastChannel<Int> = BroadcastChannel(Channel.CONFLATED)
{{< / highlight >}}

> A `conflated channel` is a channel that buffers at most one element and conflates (brings together and fuse all elements into a single one) all subsequent `send` and `offer` invocations, so that the receiver always gets the most recently sent element. That means that, if you send, for instance, 5 numbers (lets say 3, 1, 54, 34, 10) and then you start observing, you will get just the last number, in this case, the 10.

For this example I will be injecting this channel using `koin` in the folliwing way:

**ViewModule.kt**

{{< highlight Kotlin >}}
val viewModule = module {

    single<BroadcastChannel<Int>> { BroadcastChannel(Channel.CONFLATED) }
}
{{< / highlight >}}

> Notice that is a `single`, that in koin means that everytime the object is injected it will give you the same instance.

<br />

## The publisher

Next, we want to publish (send) values to the channel. For this we need to launch a new coroutine without blocking the thread, like this:

{{< highlight Kotlin >}}
GlobalScope.launch { channel.send(1) }
{{< / highlight >}}

Here we are sending the number `1` to the channel.

> The channel used here is the one that we are injecting.

To be able to send different values when we push a button, we can create a class called `ChannelPlublisher` like this:

**ChannelPublisher.kt**

{{< highlight Kotlin >}}
import kotlinx.coroutines.GlobalScope
import kotlinx.coroutines.channels.BroadcastChannel
import kotlinx.coroutines.launch
import org.koin.standalone.StandAloneContext

class ChannelPublisher {

    private val channel: BroadcastChannel<Int> = StandAloneContext.getKoin().koinContext.get()

    fun publish(element: Int) {
        GlobalScope.launch { channel.send(element) }
    }
}
{{< / highlight >}}

I will inject this class with koin so, at this moment, the koin module will be:

**ViewModule.kt**

{{< highlight Kotlin >}}
val viewModule = module {

    single<BroadcastChannel<Int>> { BroadcastChannel(Channel.CONFLATED) }
    single { ChannelPublisher() }
}
{{< / highlight >}}

We will be injecting it in this case in the `MainActivity` through the line `private val channelPublisher: ChannelPublisher by inject()`. To effectively use it, we will have the field `private var number = 0` and a button, and we will update the values doing something like this: `button.setOnClickListener { channelPublisher.publish(++number) }`.

**MainActivity.kt**

{{< highlight Kotlin >}}
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import com.siziksu.ui.R
import com.siziksu.ui.common.channel.ChannelPublisher
import kotlinx.android.synthetic.main.activity_main.mainAddButton
import org.koin.android.ext.android.inject

class MainActivity : AppCompatActivity() {

    private val channelPublisher: ChannelPublisher by inject()
    private var number = 0

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        initializeViews()
    }

    private fun initializeViews() {
        channelPublisher.publish(++number)
        mainAddButton.setOnClickListener { channelPublisher.publish(++number) }
    }
}
{{< / highlight >}}

<br />

## The observers

Now, it's time to get the values from the observers, in this case, the two fragments. To observe the channel, we need to create a coroutine and listen to the channel values like this:

{{< highlight Kotlin >}}
GlobalScope.launch { channel.consumeEach { print(it) } }
{{< / highlight >}}

And like we did before, we will create the class `ChannelObserver` to simplify the use and manipulation of the channel.

**ChannelObserver.kt**

{{< highlight Kotlin >}}
import kotlinx.coroutines.CompletableJob
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.GlobalScope
import kotlinx.coroutines.Job
import kotlinx.coroutines.channels.BroadcastChannel
import kotlinx.coroutines.channels.consumeEach
import kotlinx.coroutines.launch
import org.koin.standalone.StandAloneContext

class ChannelObserver {

    var pause = false

    private val channel: BroadcastChannel<Int> = StandAloneContext.getKoin().koinContext.get()
    private var job: CompletableJob = Job()

    fun onReceive(func: ((Int) -> Unit)) {
        GlobalScope.launch(Dispatchers.Main + job) { channel.consumeEach { if (!pause) func(it) } }
    }

    fun unsubscribe() {
        job.cancel()
    }
}
{{< / highlight >}}

> Notice here that I implemented a way to be able to pause the subscription. This comes handy when, for instance, you go to background and you don't need/want to observe while in this state.

Of course we will also inject this class, so the koin module at this point will be:

**ViewModule.kt**

{{< highlight Kotlin >}}
val viewModule = module {

    single<BroadcastChannel<Int>> { BroadcastChannel(Channel.CONFLATED) }
    single { ChannelPublisher() }
    factory { ChannelObserver() }
}
{{< / highlight >}}

> Notice that in this case the Observers are `factory`, that in koin means that everytime the object is injected it will give you a new instance.

So now, we can implement this in the observers (our fragments).

**PrimaryFragment.kt**

{{< highlight Kotlin >}}
import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.appcompat.app.AppCompatActivity
import androidx.fragment.app.Fragment
import androidx.navigation.fragment.findNavController
import com.siziksu.ui.R
import com.siziksu.ui.common.channel.ChannelObserver
import kotlinx.android.synthetic.main.fragment_primary.primaryButton
import kotlinx.android.synthetic.main.fragment_primary.primaryValue
import kotlinx.android.synthetic.main.toolbar.toolbar
import org.koin.android.ext.android.getKoin

class PrimaryFragment : Fragment() {

    private lateinit var channelObserver: ChannelObserver

    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        channelObserver = getKoin().get()
        return inflater.inflate(R.layout.fragment_primary, container, false)
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        initViews()
    }

    override fun onResume() {
        super.onResume()
        channelObserver.pause = false
    }

    override fun onPause() {
        super.onPause()
        channelObserver.pause = true
    }

    override fun onDestroy() {
        super.onDestroy()
        channelObserver.unsubscribe()
    }

    private fun initViews() {
        activity?.let { (activity as AppCompatActivity).setSupportActionBar(toolbar) }
        primaryButton.setOnClickListener { findNavController().navigate(PrimaryFragmentDirections.toSecondary()) }
        channelObserver.onReceive { primaryValue.text = getString(R.string.value, it) }
    }
}
{{< / highlight >}}

**SecondaryFragment.kt**

{{< highlight Kotlin >}}
import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.appcompat.app.AppCompatActivity
import androidx.fragment.app.Fragment
import com.siziksu.ui.R
import com.siziksu.ui.common.channel.ChannelObserver
import kotlinx.android.synthetic.main.fragment_secondary.secondaryButton
import kotlinx.android.synthetic.main.fragment_secondary.secondaryValue
import kotlinx.android.synthetic.main.toolbar.toolbar
import org.koin.android.ext.android.getKoin

class SecondaryFragment : Fragment() {

    private lateinit var channelObserver: ChannelObserver

    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        channelObserver = getKoin().get()
        return inflater.inflate(R.layout.fragment_secondary, container, false)
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        initViews()
    }

    override fun onDestroy() {
        super.onDestroy()
        channelObserver.unsubscribe()
    }

    private fun initViews() {
        activity?.let { (activity as AppCompatActivity).setSupportActionBar(toolbar) }
        secondaryButton.setOnClickListener { activity?.onBackPressed() }
        channelObserver.onReceive { secondaryValue.text = getString(R.string.value, it) }
    }
}
{{< / highlight >}}

<br />

And that's it!
