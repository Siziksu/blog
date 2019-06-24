+++
type = "post"
title = "Kotlin 'const' constants"
summary = "This post will explain the Kotlin 'const' keyword."
date = 2019-06-07T20:15:24+02:00
categories = ["kotlin"]
tags = ["constants", "kotlin"]
image = "images/abstract-architectural-art-2387660.jpg"
edit = ""
draft = false
+++
In Kotlin you have two types of constants, `const` and `val`.

__const__ are compile time constants. Meaning, that their value has to be assigned during the compile time, unlike __val__, where it can be done at runtime. This means, that _const_ can never be assigned to a function or any class constructor. Must be top-level, or member of an object declaration or a companion object.

**Declarations:**

{{< highlight Kotlin >}}
const val foo = "Hello world"
{{< / highlight >}}

**Examples:**

{{< highlight Kotlin >}}
const val TAG = "This is constant directly in the file"
{{< / highlight >}}

{{< highlight Kotlin >}}
object ConstantsObject {

    const val TAG = "This is a constant in an object"
}
{{< / highlight >}}

{{< highlight Kotlin >}}
class ConstantsClass {

    companion object {

        const val TAG = "This is a constant in a class"
    }
}
{{< / highlight >}}

**Use of this example constants:**

{{< highlight Kotlin >}}
fun main() {
    println(TAG)
    println(ConstantsObject.TAG)
    println(ConstantsClass.TAG)
}
{{< / highlight >}}
