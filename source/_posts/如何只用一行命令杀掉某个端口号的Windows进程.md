---
title: 如何只用一行命令杀掉某个端口号的Windows进程?
copyright: true
date: 2020-02-25 11:05:39
categories:
tags:
top:
---

## 如何只用一行命令杀掉某个端口号的Windows进程?

<!-- more -->

For use in command line:

```powershell
for /f "tokens=5" %a in ('netstat -aon ^| find ":8080" ^| find "LISTENING"') do taskkill /f /pid %a
```

For use in bat-file:

```powershell
for /f "tokens=5" %%a in ('netstat -aon ^| find ":8080" ^| find "LISTENING"') do taskkill /f /pid %%a
```

> If you want to do it in a .bat, replace %a for %%



转自：[Stack Overflow](https://stackoverflow.com/questions/39632667/how-do-i-kill-the-process-currently-using-a-port-on-localhost-in-windows/51099355#)