---
title: 'Windows配置LaTex及其编辑器'
description: 'Windows下配置VSCode+Texlive+LaTeX Workshop+AI辅助写作'
publishDate: '2025-09-03'
heroImage: { src: './thumbnail.jpg', color: '#64574D' }
tags: ['latex']
language: '中文'
draft: false
comment: true
---

## 一、LaTeX环境配置

### 1.TeXlive下载

TeXlive链接：http://www.tug.org/texlive/

点击[install on Windows](https://www.tug.org/texlive/windows.html)，新页面中找到[install-tl-windows.exe](https://mirror.ctan.org/systems/texlive/tlnet/install-tl-windows.exe)并点击下载

### 2.TeXlive安装

双击运行install-tl-windows.exe，按照提示点击至Install安装main installer

随后更改安装路径，进行正式安装。需下载大约9G文件，所需时间较长，请耐心等待，不要点击Abort

安装完成后点击Close，在命令行中输入`latex`检验是否安装成功

## 二、LaTeX编辑器配置

建议使用VSCode+AI插件或Cursor

### 1.安装插件

搜索并安装`LaTeX Workshop`插件和`LTeX`插件（拼写检查）

打开LaTeX Workshop插件，选择对应编译方式，编译成功后的pdf可选择在浏览器或VSCode界面中显示，重新编译后会自动刷新。

### 2.配置保存时自动格式化

1. 使用`Ctrl+,`打开VSCode设置，搜索`editor.formatOnSave`，点击勾选。
2. 搜索`latex-workshop.formatting.latex`，将该项改为`latexindent`。按下`Ctrl+s`时会自动使用latexindent格式化LaTeX文件。

### 3.配置保存时自动编译

1. VSCode设置中搜索`latex-workshop.latex.autoBuild.run`，可设置为onSave或onFileChange
2. 搜索`latex-workshop.latex.recipe.default`，将该项改为`lastUsed`，即使用上次的编译方式。第一次编译tex文件时需手动选择编译器，后面就会默认使用该编译器。

### 4.配置保存时自动清理

1. VSCode设置中搜索`latex-workshop.latex.autoClean.run`，将该项改为`onBuilt`，在编译后自动清理。
2. 搜索`latex-workshop.latex.clean.method`，将该项改为`glob`。
3. 搜索`latex-workshop.latex.clean.fileTypes`，在该项中添加需要清理的文件后缀，例如`*.synctex.gz` `*.xdv` `*.fls`。

> [2024年LaTex常见编辑器汇总：优缺点及配置教程 （Texlive/VSCode/Overleaf） - 知乎](https://zhuanlan.zhihu.com/p/607473890)
>
> [Vscode使用Latex配置代码自动格式化_vscode latex 格式化-CSDN博客](https://blog.csdn.net/qq_43153628/article/details/143161344)
>
> [Win10下配置VScode+LaTex环境及美化 - 知乎](https://zhuanlan.zhihu.com/p/367372004)
>
> [Win 下 VSCode 配置 LaTeX format 自动格式化_如何设置latex `formatting.latex`.-CSDN博客](https://blog.csdn.net/Aloneingchild/article/details/108716987)

