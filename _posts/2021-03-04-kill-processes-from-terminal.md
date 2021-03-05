---
layout: page
title:  "Kill multiple processes from terminal"
# subtitle: ""
date: 2021-03-04
tags: terminal
---

Use `pgrep` and `kill` to kill processes in terminal.  
For example, to kill all the processes of **BetterTouchTool**:
```bash
# -i to perform case-insensitive match
# kill -<signal> <pid>
pgrep -i bettertouchtool | xargs kill -9
```

For another example, to kill **FaceTimeNotificationCenterService** to turn off the camera light left by **FaceTime**.
```bash
pgrep -i FaceTimeNotificationCenterService | xargs kill -9
```

