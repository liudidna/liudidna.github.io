---
layout: page
#title:  "Markdown cheatsheet"
date:   2013-12-13 02:11:35 -0500
#categories: jekyll update
---

# Markdown cheatsheet {#top_title}

---
## Color and Font via HTML:
`<span style="color:blue">some *blue* text</span>`  
<span style="color:blue">some *blue* text</span>

`<font color="blue">some *blue* text</font>`   
<font color="blue">some *blue* text</font>

`<font color="blue", face="Courier New">some *blue* text @ Courier New</font>`  
*(Not supported by DP)*  
<font color="blue", face="Courier New">some *blue* text @ Courier New</font>

`<span style="color:blue; font-family:'Courier New'; font-size:40px">some 40px *blue* text @ Courier New</span>`  
<span style="color:blue; font-family:'Courier New'; font-size:40px">some 40px *blue* text @ Courier New</span> is here

`<font color="blue"><i>some</i> <u>blue</u> <b>text</b></font>`  
<font color="blue"><i>some</i> <u>blue</u> <b>text</b></font>  

---

## Space:
`some&nbsp;&nbsp;&nbsp;&nbsp;spaces`  
some&nbsp;&nbsp;&nbsp;&nbsp;spaces

---

## Line break:
`line<br>break`  
line<br>break

## link & picture:
#### link method 1:
```bash
[Google1][]
[Google1]: http://google.com/
```
*(Not supported by DP)*  

[Google1][]  
[Google1]: http://google.com/
do not foget `http://`
#### link method 2:
```
[Google2][12]
[12]: http://google.com/
```
*(Not supported by DP)*  
[Google2][12]  
[12]: http://google.com/
#### link method 3:
`[Google3](http://google.com/)`  
[Google3](http://google.com/)
no space between `]` and `(`

#### link method 4 (open in new tab):
`<a href="http://google.com/" target="_blank">Go</a>`  
<a href="http://google.com/" target="_blank">Go</a>

#### link to heading IDs:
*(Not supported by DP)*  
`# Markdown cheatsheet {#top_title}`  
[To top](#top_title)

#### linked image 1:
**After you copy the public Dropbox link to your clipboard, just change `?dl=0` to `?raw=1` at the end of the URL.**  
`![myself](https://www.dropbox.com/s/jbn7jwu7uw4ovpa/0431%2520ld.jpg?raw=1)`  
![myself](https://www.dropbox.com/s/jbn7jwu7uw4ovpa/0431%2520ld.jpg?raw=1)

#### linked image 2:
`<img src="https://www.dropbox.com/s/jbn7jwu7uw4ovpa/0431%2520ld.jpg?raw=1" width = "50" height = "100" alt="title" align=center>`  
<img src="https://www.dropbox.com/s/jbn7jwu7uw4ovpa/0431%2520ld.jpg?raw=1" width = "50" height = "100" alt="title" align=center>

#### linked image 3:
```HTML
<div style="text-align: center">
<p>Handsome man. <img src="https://www.dropbox.com/s/jbn7jwu7uw4ovpa/0431%2520ld.jpg?raw=1" width = "50" height = "100" alt="title" align=center> Yes he is.</p>
<p>Handsome man. <img src="https://www.dropbox.com/s/jbn7jwu7uw4ovpa/0431%2520ld.jpg?raw=1" width = "50" height = "100" alt="title" align=top> Yes he is.</p>
</div>
```
<div style="text-align: center">
<p>Handsome man. <img src="https://www.dropbox.com/s/jbn7jwu7uw4ovpa/0431%2520ld.jpg?raw=1" width = "50" height = "100" alt="title" align=center> Yes he is.</p>
<p>Handsome man. <img src="https://www.dropbox.com/s/jbn7jwu7uw4ovpa/0431%2520ld.jpg?raw=1" width = "50" height = "100" alt="title" align=top> Yes he is.</p>
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
*(Not supported by DP)*  


| old version | new version |
|:---:|:---:|
|`C5*`           | `C5'`|
|`..*`           | `..'`|
|`O1P`          |`OP1`|
|`O2P`          |`OP2`|

For convenience, use the [`TablesGenerator`](https://www.tablesgenerator.com/markdown_tables)!

---
## Comments
`[//]: # (This may be the most platform independent comment)`  
*(Not supported by DP)*  
[//]: # (This may be the most platform independent comment)

---
## Useful links:
[Wiznote](https://www.wiz.cn/feature-markdown.html)  
[huihut](https://blog.huihut.com/2017/01/25/MarkdownTutorial/)  
[Markdown Guide](https://www.markdownguide.org)

---

[To top](#top_title)