---
layout: post
title: Markdown Reference
date: '2018-05-24T17:57:02.002-07:00'
author: Weishi Zeng
tags: tools
---

# Markdown Reference

This document contains some of the most commonly used markdown syntax.
:fire:


## Line Break
Markdown treat new line in source file
as space.   
A new line can be achieved by [two space + hit enter]

A new paragraph is achieved by an empty line. [hit enter twice]

## Text Style
*italic* or _italic_  
**bold** or __bold__  
*italic **and** bold*  
~~strike through~~  

## Text Block
A quote block using `>`.
> I’ve always been more interested
> in the future than in the past.

A code block using \`\`\`.
```
The Clarke and Wright Algorithm
  pick a start point
  calculate savings for all nodes
  sort the savings
  try merge from top
```

A code block using [empty line + 4 space].  

    block of
    text
    in Shakespeare's
    style

A code block with color.
```javascript
function test() {
 console.log("Need car wash.");
}
```

A inline code piece.  
Let's try `select * from table;`

## Table

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

[comment]:# (comments are ignored.)

Colons can be used to align columns.  
The outer pipes (|) are optional.
There must be at least 3 dashes separating each header cell.

## List

Use `*` `-` `+` for unordered list.
- banana
+ o-range
* apple

Use 1,2,3 for ordered list.
1. item1
2. item2, this can be any number
3. item3

Use indent for sublist.
* food
 * fruit (1 space)
   * apple (2 space)
     * fuji apple
* water

Use indent for ordered list
1. section 1
 1. section 1.A (1 space)
    1. section 1.A.a (3 space, don't know why)
       1. section 1.A.a.I (3 space)  
       This sub section lines up well.

Use `[X]` for todo list
- [x] this is a complete item
- [ ] this is an incomplete item

## Link
Links are automatic.  
This link is clickable http://github.com  
You can visit [GitHub](http://github.com).

## Picture
Pictures are links with prefix of `!`.  

Do it like a link:  
![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1")

Define a url and refer to it:  
![alt text][logo]

[logo]: https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 2"

## Escape
Use \ for escape special characters. (\*, \`, etc)

## Header

Use # 1 - 6 times for headers.
* # This is an <h1\> tag
* ## This is an &lt;h2&gt; tag
* ###### This is an <h6\> tag


## Emoji

:kissing_closed_eyes:
:kissing_smiling_eyes:
:grinning:
:grin:
:satisfied:

:flushed:
:relieved:
:sob:

:broken_heart:
:shit:
:fire:

:scream:
:joy:

:+1:
:ok_hand:
