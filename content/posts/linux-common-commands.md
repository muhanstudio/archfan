---
title: "Linux 的常用命令"
date: 2024-01-01
draft: false
tags: ["Linux", "命令行"]
---

## 基础操作命令
### 查看文件和目录
- `ls`：列出目录内容。例如，`ls -l` 可以显示详细的文件信息。
- `pwd`：显示当前工作目录的完整路径。
- `cd`：切换目录。如 `cd /home/user` 可以进入用户主目录。

### 文件操作
- `touch`：创建新文件。例如，`touch test.txt` 会创建一个名为 `test.txt` 的文件。
- `cp`：复制文件或目录。`cp source.txt destination/` 可以将 `source.txt` 复制到 `destination` 目录下。
- `mv`：移动文件或重命名文件。`mv old.txt new.txt` 可以将 `old.txt` 重命名为 `new.txt`。

### 文本处理
- `cat`：查看文件内容。`cat file.txt` 会将 `file.txt` 的内容输出到终端。
- `grep`：在文件中查找指定的字符串。`grep "keyword" file.txt` 会在 `file.txt` 中查找包含 `keyword` 的行。