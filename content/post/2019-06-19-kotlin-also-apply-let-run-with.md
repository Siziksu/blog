+++
type = "post"
title = "Kotlin also, apply, let, run and with"
summary = "This post will try to show the differences between also, apply, let, run and with in Kotlin."
date = 2019-06-19T19:50:14+02:00
categories = ["kotlin"]
tags = ["also", "apply", "let", "run", "with"]
image = "images/architectural-design-architecture-buildings-830891.jpg"
edit = ""
draft = false
+++
> **NOTE**: For this post, we will be using the following object

{{< highlight Kotlin >}}
class Person(val id: Int, var name: String = "Unknown", var age: Int = 0) : Comparable<Person> {

    override fun compareTo(other: Person): Int {
        return if (this.name > other.name) 1 else if (this.name < other.name) -1 else 0
    }

    override fun toString(): String = "$id. $name $age"
}
{{< / highlight >}}

<br />

## What they are

Function | What is it | Context | Return value
--- | --- | --- | ---
**also** | Extension | **it** | Context object 
**apply** | Extension | **this** | Context object
**let** | Extension | **it** | The lambda result
**run** | Extension | **this** | The lambda result
**with** | Function | **this** | The lambda result
**run** | Function | **-** | Block result

<br />

## Also and Apply

The extension functions `also` and `apply` **return** the `context object`, meaning that can be used to chain steps over the same object, and also, be the return for functions returning the context object.

{{< highlight Kotlin >}}
fun main() {
    val list = getList()
    list.sort()
    list.forEach { println(it) }
    // 2. Markus 33
    // 3. Ron 19
    // 1. Rowan 31
}

fun getList(): ArrayList<Person> = ArrayList<Person>().also {
    println("The list has ${it.size} items") // The list has 0 items
}.apply {
    add(Person(1, "Rowan", 31))
    add(Person(2, "Markus", 33))
    add(Person(3, "Ron", 19))
}.also {
    println("The list has ${it.size} items") // The list has 3 items
}
{{< / highlight >}}

<br />

## Let, Run and With

The functions `let`, `run` and `with` **return** the `lambda result`, meaning that you can return nothing or whatever you need.

{{< highlight Kotlin >}}
val str: String? = "Three"
str?.run {
    println("The length of the received string is: $length")
    length * 2
}?.let {
    println("The number received is $it")
} ?: println("The string is null")
// The length of the received string is: 5
// The number received is 10
{{< / highlight >}}

{{< highlight Kotlin >}}
val person = Person(1)
with(person) {
    name = "Rowan"
    age = 31
    age
}.also { println("The age of this person is $it") } // The age of this person is 31
println(person) // 1. Rowan 31
{{< / highlight >}}

You can use nullability in `with` using `let`.

{{< highlight Kotlin >}}
val person: Person? = null
with(person) {
    this?.let {
        name = "Rowan"
        age = 31        
        println(person)
    } ?: println("The object person is null")
}
// The object person is null
{{< / highlight >}}

## Run

The function `run` can be also used as a non-extension function. This lets you execute a block of code where an expression is required. In this blocks you can return nothing or whatever you need.

{{< highlight Kotlin >}}
val hexNumberRegexPattern = run {
    val digits = "0-9"
    val hexDigits = "A-Fa-f"
    val sign = "+-"

    Regex("[$sign]?[$digits$hexDigits]+")
}
println(hexNumberRegexPattern) // [+-]?[0-9A-Fa-f]+
{{< / highlight >}}