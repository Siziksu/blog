+++
type = "post"
title = "Markdown Syntax Guide"
summary = "This post is just for testing purposes for this Hugo theme."
date = 2019-06-01T13:30:48+02:00
categories = ["markdown"]
tags = ["markdown"]
image = "random/4.jpg"
edit = ""
draft = false
+++

**Titles**

---

# Title 1

This is the text.

## Title 2

This is the text.

### Title 3

This is the text.

#### Title 4

This is the text.

##### Title 5

This is the text.

###### Title 6

This is the text.

**Paragraph**

This is the first paragraph.


**Emphasis**

---
Emphasis, aka italics, with *asterisks* or _underscores_.  
Strong emphasis, aka bold, with **asterisks** or __underscores__.  
Combined emphasis with **asterisks and _underscores_**.  
Strikethrough uses two tildes. ~~Scratch this.~~

**Lists**

---

1. First ordered list item
2. Another item
  * Unordered sub-list. 
1. Actual numbers don't matter, just that it's a number
  1. Ordered sub-list
4. And another item.  
You can have properly indented paragraphs within list items. Notice the blank line above, and the leading spaces (at least one, but we'll use three here to also align the raw Markdown).  
To have a line break without a paragraph, you will need to use two trailing spaces.
Note that this line is separate, but within the same paragraph.
(This is contrary to the typical GFM line break behaviour, where trailing spaces are not required.)

Unordered list can use asterisks (*), minuses (-) or pluses (+).

**Links**

---

There are two ways to create links.

[I'm an inline-style link](https://www.example.com)

[I'm an inline-style link with title](https://www.example.com "Example page")

[I'm a reference-style link][Arbitrary case-insensitive reference text]

[I'm a relative reference to a repository file](../images/markdown.png)

[You can use numbers for reference-style link definitions][1]

Or leave it empty and use the [link text itself].

URLs and URLs in angle brackets will automatically get turned into links. 
https://www.example.com or <https://www.example.com> and sometimes 
example.com (but not on Github, for example).

Some text to show that the reference links can follow later.

[arbitrary case-insensitive reference text]: https://www.example.com
[1]: https://example.com
[link text itself]: https://www.example.com

**Images**

---

Here's our logo (hover to see the title text):

Inline-style: 
![alt text](../images/markdown.png "Markdown logo")

Reference-style: 
![alt text][logo]

[logo]: ../images/markdown.png "Markdown logo"

**Code and Syntax Highlighting**

---

Inline `code` has `back-ticks around` it.

```javascript
var s = "JavaScript syntax highlighting";
alert(s);
```

```python
s = "Python syntax highlighting"
print s
```

```
No language indicated, so no syntax highlighting. 
But let's throw in a <b>tag</b>.
```

**Tables**

---

Tables aren't part of the core Markdown spec, but they are part of GFM and Markdown Here supports them. They are an easy way of adding tables to your email -- a task that would otherwise require copy-pasting from another application.

Colons can be used to align columns.

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

There must be at least 3 dashes separating each header cell.
The outer pipes (|) are optional, and you don't need to make the 
raw Markdown line up prettily. You can also use inline Markdown.

Markdown | Less | Pretty
--- | --- | ---
*Still* | `renders` | **nicely**
1 | 2 | 3

**Blockquotes**

---

> Blockquotes are very handy in email to emulate reply text.
> This line is part of the same quote.

Quote break.

> This is a very long line that will still be quoted properly when it wraps. Oh boy let's keep writing to make sure this is long enough to actually wrap for everyone. Oh, you can *put* **Markdown** into a blockquote.

**Horizontal Rule**

---

Three or more minuses (-), hyphens or asterisks (*).

**YouTube Videos**

<iframe width="1280" height="720" src="https://www.youtube.com/embed/HANCzu70us4" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
