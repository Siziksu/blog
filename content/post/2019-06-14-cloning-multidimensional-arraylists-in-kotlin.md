+++
type = "post"
title = "Cloning Multidimensional ArrayLists in Kotlin"
summary = "This post will explain, with an example, how to clone a complex ArrayList in Kotlin without the need to serialize/deserialize."
date = 2019-06-14T17:56:50+02:00
categories = ["kotlin"]
tags = ["kotlin", "arraylist", "clone"]
image = "images/adventure-beautiful-dawn-2387418.jpg"
edit = ""
draft = false
+++
This is a very fast and efficient way to clone complex ArrayLists. This way you can avoid the use of serializing/deserizlizing, that will have a hard impact compared with this solution.

{{< highlight Kotlin >}}
import com.simfy.core.UnitTest
import org.junit.Assert.assertEquals
import org.junit.Test

class ArrayListsCloneTests : UnitTest() {

    @Test
    fun `test 1 cloning an array list of array lists`() {
        val original = "original"
        val mutated = "mutated"

        val list = arrayListOf(arrayListOf(original))

        val clone = list.map { it.toMutableList() as ArrayList<String> }

        clone[0][0] = mutated

        assertEquals(original, list[0][0])
        assertEquals(mutated, clone[0][0])
    }

    @Test
    fun `test 2 cloning an array list of objects that include an array list`() {
        val original = "original"
        val mutated = "mutated"

        val list = arrayListOf(TestObject(arrayListOf(original)))

        val clone = list.map { TestObject(it.list.toMutableList() as ArrayList<String>) }

        clone[0].list[0] = mutated

        assertEquals(original, list[0].list[0])
        assertEquals(mutated, clone[0].list[0])
    }
}

data class TestObject(val list: ArrayList<String>)
{{< / highlight >}}