---
layout: page
title:  "Markdown cheatsheet"
# subtitle: ""
date: 2013-12-13 02:11:35 -0500
tags: markdown website
---

## Contents {#top_title}
{: .no_toc}

1. TOC
{:toc}

---

## Color and Font via HTML and inline CSS:

`<span style="color:blue">some *blue* text</span>`  
<span style="color:blue">some _blue_ text</span>

`<font color="blue">some *blue* text</font>`  
<font color="blue">some _blue_ text</font>

`<font color="blue", face="Courier New">some *blue* text @ Courier New</font>`  
_(Not supported by DP)_  
<font color="blue", face="Courier New">some _blue_ text @ Courier New</font>

`<span style="color:blue; font-family:'Courier New'; font-size:40px">some 40px *blue* text @ Courier New</span>`  
<span style="color:blue; font-family:'Courier New'; font-size:40px">some 40px _blue_ text @ Courier New</span> is here

`<font color="blue"><i>some</i> <u>blue</u> <b>text</b></font>`  
<font color="blue"><i>some</i> <u>blue</u> <b>text</b></font>

`<sup>A</sup> A <sub>B</sub>`  
<sup>A</sup> A <sub>B</sub>

`<span style="color:green" onMouseOver="this.style.color='red'; this.style.fontSize='2em';" onMouseOut="this.style.color='blue'; this.style.fontSize='1em'">some changing text</span>`  
<span style="color:green" onMouseOver="this.style.color='red'; this.style.fontSize='2em';" onMouseOut="this.style.color='blue'; this.style.fontSize='1em'">some changing text</span>

`<kbd>Command</kbd> + <kbd class="">T</kbd>`  
<kbd>Command</kbd> + <kbd class="">T</kbd>
<!-- # some changing text {:style="color:green"} -->

<!-- **with kramdown syntax** -->
<!-- <style>
    .blue_bold {
        color: blue;
        font-weight: bold;
    }
</style>

#### A blue and bold paragraph. { .blue_bold} -->

## Special separators via HTML:
**Space:**  
`some&nbsp;&nbsp;&nbsp;&nbsp;spaces`  
some&nbsp;&nbsp;&nbsp;&nbsp;spaces

**Line break:**  
`line<br>break`  
line<br>break

**horizontal line:**  
`<hr>`
<hr>

---

## link & picture:

**link method 1:**

```
[Google1][]

[Google1]: http://google.com/
```

_(Not supported by DP, better to add an empty line)_

[Google1][]  

[Google1]: http://google.com/  
do not foget `http://`

**link method 2:**

```
[Google2][12]

[12]: http://google.com/
```

_(Not supported by DP, better to add an empty line)_  
[Google2][12]  

[12]: http://google.com/

**link method 3:**  
`[Google3](http://google.com/)`  
[Google3](http://google.com/)
no space between `]` and `(`

**link method 4 (open in new tab):**  
`<a href="http://google.com/" target="_blank">Go</a>`  
<a href="http://google.com/" target="_blank">Go</a>

**link method 5 (open in new tab2, kramdown):**  
`[Go2](http://google.com/){:target="_blank"}`  
[Go2](http://google.com/){:target="\_blank"}

**link to heading IDs:**  
_(Not supported by DP)_  
`# Markdown cheatsheet {#top_title}`  
`[To top](#top_title)`  
[To top](#top_title)

**linked image 1:**  
**After you copy the public Dropbox link to your clipboard, just change `?dl=0` to `?raw=1` at the end of the URL.**  
`![myself](https://www.dropbox.com/s/jbn7jwu7uw4ovpa/0431%2520ld.jpg?raw=1)`  
![myself](https://www.dropbox.com/s/jbn7jwu7uw4ovpa/0431%2520ld.jpg?raw=1)

**linked image 2, with HTML:**  
`<img src="https://www.dropbox.com/s/jbn7jwu7uw4ovpa/0431%2520ld.jpg?raw=1" width = "50" height = "100" alt="title">`  
<img src="https://www.dropbox.com/s/jbn7jwu7uw4ovpa/0431%2520ld.jpg?raw=1" width = "50" height = "100" alt="title">

**linked image 3, with HTML more options:**

```HTML
<div style="text-align: center">
<p>Young man. <img src="https://www.dropbox.com/s/jbn7jwu7uw4ovpa/0431%2520ld.jpg?raw=1" width = "50" height = "100" alt="title" style="vertical-align:middle"> Yes he was.</p>
<p>Young man. <img src="https://www.dropbox.com/s/jbn7jwu7uw4ovpa/0431%2520ld.jpg?raw=1" width = "50" height = "100" alt="title" style="vertical-align:top"> Yes he was.</p>
</div>
```

<div style="text-align: center">
<p>Young man. <img src="https://www.dropbox.com/s/jbn7jwu7uw4ovpa/0431%2520ld.jpg?raw=1" width = "50" height = "100" alt="title" style="vertical-align:middle"> Yes he was.</p>
<p>Young man. <img src="https://www.dropbox.com/s/jbn7jwu7uw4ovpa/0431%2520ld.jpg?raw=1" width = "50" height = "100" alt="title" style="vertical-align:top"> Yes he was.</p>
</div>

---

## Align

`<div style="text-align: right"> your-text-here </div>`

<div style="text-align: right"> your-text-here </div>
`<div style="text-align: justify"> your-text-here </div>`  
<div style="text-align: justify"> your-text-here </div>

---

## Table

```
| old version | new version |
|:---:|:---:|
|`C5*`           | `C5'`|
|`..*`           | `..'`|
|`O1P`          |`OP1`|
|`O2P`          |`OP2`|
```

_(Not supported by DP)_

| old version | new version |
| :---------: | :---------: |
|    `C5*`    |    `C5'`    |
|    `..*`    |    `..'`    |
|    `O1P`    |    `OP1`    |
|    `O2P`    |    `OP2`    |

For convenience, use the [`TablesGenerator`](https://www.tablesgenerator.com/markdown_tables)!

---

## Comments

`[//]: # (This may be the most platform independent comment)`  
_(Not supported by DP)_  
[//]: # (This may be the most platform independent comment)

`<!-- <p>HTML comments</p> -->`
_(It works if nothing appears below.)_

<!-- <p>HTML comments</p> -->

---

## TOC

**not ordered**

```
* TOC
{:toc}
```

**ordered**

```
1. TOC
{:toc}
```

## Expand

```
<details>
<summary>Click to expand!</summary>

- A
- B

</details>
```

_(MD syntax may not be recognized inside HTML block)_

<details>
<summary>Click to expand!</summary>

- A
- B

</details>

---
## Footnotes:
```
This is some text[^1].

[^1]: Some *crazy* footnote definition.
```
*(Sometimes an empty line is needed.)*  
This is some text[^1].

[^1]: Some *crazy* footnote definition.


## Useful links:

[Wiznote](https://www.wiz.cn/feature-markdown.html)  
[huihut](https://blog.huihut.com/2017/01/25/MarkdownTutorial/)  
[Markdown Guide](https://www.markdownguide.org)  
[kramdown](https://kramdown.gettalong.org/index.html)  
[Github writing](https://docs.github.com/en/free-pro-team@latest/github/writing-on-github/basic-writing-and-formatting-syntax)

---

<a href="#top_title">
    <button style="position: fixed; top: 90%; right: 10%; border-radius: 50%; padding: 0.5em 1em;"><i class="fas fa-sync"></i> To top</button>
</a>
