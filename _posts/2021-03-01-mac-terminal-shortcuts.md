---
layout: page
title:  "Mac terminal shortcuts"
# subtitle: ""
date: 2021-03-01
tags: mac terminal
---

## Moving
<kbd>Option</kbd> + <kbd>Left</kbd> / <kbd>Right</kbd>:  
Move the cursor between separate words in a command line.

<kbd>Control</kbd> + <kbd>A</kbd>:  
Back to the beginning, or the far left, of the line.

<kbd>Control</kbd> + <kbd>E</kbd>:  
Back to the end, or the far right, of the line.

<kbd>Control</kbd> + <kbd>X</kbd> + <kbd>X</kbd>:  
Toggle between the start of line and current cursor position.


## Editing
<kbd>Control</kbd> + <kbd>W</kbd>:  
Delete the word immediately before the cursor.

<kbd>Control</kbd> + <kbd>U</kbd>:  
Clear the entirety of the line before the cursor.

<kbd>Control</kbd> + <kbd>K</kbd>:  
Clear the entirety of the line after the cursor.

<kbd>esc</kbd> + <kbd>T</kbd>:  
Swap the two words that appear immediately before the cursor. So, if "this is" sits before the cursor, using Escape and T will change that to "is this."

<kbd>Control</kbd> + <kbd>T</kbd>:  
Swap the last two characters before the cursor.

## control or history
<kbd>Control</kbd> + <kbd>R</kbd>:  
Locate a previously used command in Terminal.

<kbd>Control</kbd> + <kbd>C</kbd>:  
Abort/kill the current application.

<kbd>Control</kbd> + <kbd>D</kbd>:  
Exit the current shell in Terminal.

<kbd>Control</kbd> + <kbd>Z</kbd>:  
Suspend what you are currently running in the background.

`!!`:  
Execute the last command entered; using `sudo !!` for permission issues.

`top`:  
Display all of your active processes. Similar to what you'd get from Activity Monitor, but within Terminal. Press "Q" to quit.

`history + a number`:  
`history 5` would show you the last five commands you typed.


## References:
[ss64](https://ss64.com/osx/syntax-bashkeyboard.html){:target="_blank"}
[techrepublic](https://www.techrepublic.com/article/20-terminal-shortcuts-developers-need-to-know/){:target="_blank"}
