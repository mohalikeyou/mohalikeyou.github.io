---
title : 重装windows
data : 2022/9/30
tags : 摆弄
---

    本贴记录了本人在重装Windows系统后需要注意的事项！

# 小技巧

1. 用户名一定要英文

2. 使用专业版

3. 惠普快速启动的快捷键为ESC

4. 更改新文件的安装位置

5. 软件和系统盘尽量分开，因为直接在系统盘装软件，可能需要部分权限，不方便

6. 安装好一定的软件之后，记得创建好还原点，以后可以还原

7. 取消打开每个软件的通知

8. 不要把全局字符集都设置为utf8，因为部分国内软件不兼容。

9. WSL中的daemon的端口和本地端口共享，也就是说本机访问WSL的daemon是直接访问的。

# 必装软件

1. directX增强版，修复Windows常用的dll库

2. 7z，最强解缩软件

3. 印象笔记

4. 坚果云，同步软件

5. 知云文献翻译，看文献

6. XTranslator，全局翻译

7. everything，搜索软件

8. utool，效率工具，必装！

9. potplayer64，视频工具

10. Clash For Windows

11. windows terminal

12. ohmyposh，美化powershell和zsh；

13. git

14. Imtip

15. MyComputerManager，删除各种同步空间

16. 新版powershell，简写为pwsh，位于`C:\Users\yao\AppData\Local\Microsoft\WindowsApps\pwsh.exe`，AppData是一个隐藏文件夹；
- 为powershell安装自动补全和智能提示

- 安装scoop包管理器
17. 360安全卫士极速版，软件管家，360文件夹，360驱动大师
- 如果老是对某类文件报错，就把整个文件夹都加入信任列表
