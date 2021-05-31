---
layout: post
title: 配置 VSCode 作为LaTex 编辑器
date: 2020-10-07
tags: [工具]
---

# 1.安装texlive
[Windows系统下latex：texlive2016和texstudio](https://jingyan.baidu.com/article/b2c186c83c9b40c46ff6ff4f.html)
texstudio 相当于编程时的IDE，可以使用这个经验贴安装texstudio，然后在texstudio中编写tex文档。所以如果你准备用VSCode来编写tex文档的话，可以不用安装texstudio。
# 2.安装并配置VSCode
## 2.1 安装VSCode
[VSCode下载](https://code.visualstudio.com/docs/?dv=win)
next，next就安装好了。

## 2.2 配置VSCode

(1)安装**LaTeX Workshop**插件
![](/images/posts/2021-05-31-21-28-57.png)

(2)修改**user setting**
从$文件\to 首选项 \to 设置$进入用户设置
![](/images/posts/2021-05-31-21-29-21.png)

点击上图标记的部分，选择**打开settings.json**

![](/images/posts/2021-05-31-21-29-50.png)
配置文件如下：
```

{
  "latex-workshop.latex.recipes": [{
  "name": "xelatex",
  "tools": [
      "xelatex"
  ]
}, {
  "name": "latexmk",
  "tools": [
      "latexmk"
  ]
},

{
  "name": "pdflatex -> bibtex -> pdflatex*2",
  "tools": [
      "pdflatex",
      "bibtex",
      "pdflatex",
      "pdflatex"
  ]
}
],
"latex-workshop.latex.tools": [{
"name": "latexmk",
"command": "latexmk",
"args": [
  "-synctex=1",
  "-interaction=nonstopmode",
  "-file-line-error",
  "-pdf",
  "%DOC%"
]
}, {
"name": "xelatex",
"command": "xelatex",
"args": [
  "-synctex=1",
  "-interaction=nonstopmode",
  "-file-line-error",
  "%DOC%"
]
}, {
"name": "pdflatex",
"command": "pdflatex",
"args": [
  "-synctex=1",
  "-interaction=nonstopmode",
  "-file-line-error",
  "%DOC%"
]
}, {
"name": "bibtex",
"command": "bibtex",
"args": [
  "%DOCFILE%"
]
}],
"latex-workshop.view.pdf.viewer": "tab",
"latex-workshop.latex.clean.fileTypes": [
"*.aux",
"*.bbl",
"*.blg",
"*.idx",
"*.ind",
"*.lof",
"*.lot",
"*.out",
"*.toc",
"*.acn",
"*.acr",
"*.alg",
"*.glg",
"*.glo",
"*.gls",
"*.ist",
"*.fls",
"*.log",
"*.fdb_latexmk"
],
}
```
重启VSCode
# 3.简单使用
(1)打开文件夹，之后所有产生的文件都在此文件夹下

![](/images/posts/2021-05-31-21-30-19.png)

(2)新建tex文件

![](/images/posts/2021-05-31-21-30-42.png)

扩展名是**.tex**

(3)编写tex文档

在**test.tex**中输入：
```
\documentclass[UTF8]{ctexart}
    \usepackage{ctex}
\begin{document}
你好，Latex
\end{document}
```

![](/images/posts/2021-05-31-21-31-15.png)

![](/images/posts/2021-05-31-21-31-47.png)

预览可以选择在VSCode的标签页中打开，也可以选择在浏览器中打开。或者点击右上角的预览按钮。
**注意：当你更改了tex文档的时候，需要先进行编译，然后才能预览，否则内容还是修改之前的内容**

预览的效果：

![](/images/posts/2021-05-31-21-32-03.png)


