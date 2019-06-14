+++
title = "Welcome post"
description = "Introduction to the blog."
date = 2019-06-02T21:30:50+02:00
categories = ["welcome"]
tags = ["welcome"]
image = "images/grass-nature-plants-12141.jpg"
edit = ""
draft = false
+++
Well, this is my first blog. I always wanted to have one, but you know, never is the right moment to sit down and spend some time writing.

Finally, I decided to do it. Maybe I'm aging and slowing down my pace of life a bit ... anyways, I hope you'll find here some useful posts.

{{< highlight Kotlin >}}
class SiziksuBlog : HugoBlog {

    private val posts = ArrayList<Post>()

    val count: Int
        get() = posts.size

    fun showPosts(list: List<Post>, onDone: () -> Unit) {
        posts.clear()
        posts.addAll(list)
        onDone()
    }

    fun getPost(position: Int): Post = posts.getOrNull(position)
}
{{< / highlight >}}
