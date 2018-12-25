---
layout: post
title: "Data Visualization - Dot Lang & Graphviz"
description: "数据可视化-Dot语言与Graphviz"
categories: [Data-Mining]
tags: [数据挖掘, 数据可视化, dot, graphviz]
redirect_from:
  - /2016/12/28/
---

* Kramdown table of contents
{:toc .toc}

Data Visualization - Dot Lang & Graphviz
==========

# Graphviz安装
依据[官网](http://www.graphviz.org/Download..php)讲述的下载、安装方式进行。

**Windows**:
下载：[http://www.graphviz.org/pub/graphviz/stable/windows/graphviz-2.38.msi](http://www.graphviz.org/pub/graphviz/stable/windows/graphviz-2.38.msi)
以管理员权限运行 graphviz-x.xx.msi
> Warning for Vista users: Even if you are logged in as adminstrator, double-clicking on the MSI file or running the MSI file from a command prompt may still not may not provide sufficient privileges. You have to run "msiexec /a graphviz-x.xx.msi".[^f1]


配置环境变量，将X:\PATH\_TO\_Graphviz2.38\bin加入到PATH中
> Note: As of version 2.31, the Visual Studio package no longer alters the PATH variable or accesses the registry at all. If you wish to use the command-line interface to Graphviz or are using some other program that calls a Graphviz program, you will need to set the PATH variable yourself.[^f2]

**Redhat/Centos**:
使用yum Repository
设置yum源 graphviz-rhel.repo 到 /etc/yum.repos.d/
执行安装
```
yum list available 'graphviz*'
yum install 'graphviz*'
```
或者下载各个组件的rpm安装包进行安装，参考http://www.graphviz.org/Download_linux_rhel.php

**Ubuntu**:
目前有一个graphviz开发版可用于apt-get 安装，参考https://launchpad.net/~gviz-adm/+archive/ubuntu/graphviz-dev
或者下载各个组件的deb安装包进行安装，参考http://www.graphviz.org/Download_linux_ubuntu.php


***Anaconda ***(如果你使用Python则推荐这种方式):
不管是Linux, windows, osx操作系统，安装好anaconda之后运行
```
conda install -c anaconda graphviz=2.38.0
```
即可完成安装。
> graphviz conda package 并不是 Python package，如果使用import graphviz则会报出" no module named graphviz "错误，这便是因为graphviz conda package 仅是将lib files放入了library/中，例如dot可以在"/PATH\_TO\_ANACONDA/bin/"目录中找到并执行。
> 如果要安装graphviz Python package, 使用"pip install graphviz"[^f3]

最近使用yum安装时出现错误
![这里写图片描述](/upload_imgs/20161228011732826.png)
目前这个BUG已经被提交，还没有进一步更新。[^f4][^f5]

安装完成后执行
```
dot -version
```
显示graphviz相关的版本信息
![这里写图片描述](/upload_imgs/20161227172653302.png)

还可以打开graphviz的编辑器gvedit
```
gvedit
```
![这里写图片描述](/upload_imgs/20161228011513544.png)


<br>
## DOT语言
DOT (graph description language)
DOT是一种用文本文件表示的图像描述语言，一般DOT文件以"gv"或者"dot"为后缀（"gv"的表示方式是用来跟早期(2007之前)的Microsoft  Word区分）。[^f6]

## Syntax
DOT 的抽象语法表达[^f7]
```
     graph	:	[ strict ] (graph | digraph) [ ID ] '{' stmt_list '}'
 stmt_list	:	[ stmt [ ';' ] stmt_list ]
      stmt	:	node_stmt
            |	edge_stmt
			|	attr_stmt
			|	ID '=' ID
			|	subgraph
 attr_stmt	:	(graph | node | edge) attr_list
 attr_list	:	'[' [ a_list ] ']' [ attr_list ]
    a_list	:	ID '=' ID [ (';' | ',') ] [ a_list ]
 edge_stmt	:	(node_id | subgraph) edgeRHS [ attr_list ]
   edgeRHS	:	edgeop (node_id | subgraph) [ edgeRHS ]
 node_stmt	:	node_id [ attr_list ]
   node_id	:	ID [ port ]
      port	:	':' ID [ ':' compass_pt ]
			|	':' compass_pt
  subgraph	:	[ subgraph [ ID ] ] '{' stmt_list '}'
compass_pt	:	(n | ne | e | se | s | sw | w | nw | c | _)
```
可以参考wiki中的几个例子来解释DOT语法。[^f8]  
**Undirected graphs (无向图)**  
**Directed graphs (有向图)**  
**Attributes (属性)**  
属性字典请参考:[http://www.graphviz.org/content/attrs](http://www.graphviz.org/content/attrs)  
**Comments (注释)**  
更多示例可以参考：  
[http://blog.csdn.net/zhangskd/article/details/8250470](http://blog.csdn.net/zhangskd/article/details/8250470)  
[http://www.graphviz.org/Gallery.php](http://www.graphviz.org/Gallery.php)  

## Layout programs
The DOT language defines a graph, but does not provide facilities for rendering the graph. There are several programs that can be used to render, view, and manipulate graphs in the DOT language:

Graphviz - A collection of libraries and utilities to manipulate and render graphs  
Canviz - a JavaScript library for rendering dot files.  
Viz.js - A simple Graphviz JavaScript client  
Grappa - A partial port of Graphviz to Java.  
Beluging - A Python & Google Cloud based viewer of DOT and Beluga extensions.  
Tulip can import dot files for analysis  
OmniGraffle can import a subset of DOT, producing an editable document. (The result cannot be exported back to DOT, however.)  
ZGRViewer, a GraphViz/DOT Viewer link  
VizierFX, A Flex graph rendering library link  
Gephi - an interactive visualization and exploration platform for all kinds of networks and complex systems, dynamic and hierarchical graphs  

未完待续......


[^f1]: [http://www.graphviz.org/Download_windows.php](http://www.graphviz.org/Download_windows.php)

[^f2]: [http://www.graphviz.org/Download_windows.php](http://www.graphviz.org/Download_windows.php)

[^f3]: [http://stackoverflow.com/questions/33433274/anaconda-graphviz-cant-import-after-installation](http://stackoverflow.com/questions/33433274/anaconda-graphviz-cant-import-after-installation)

[^f4]: [http://www.graphviz.org/mantisbt/print_bug_page.php?bug_id=2453](http://www.graphviz.org/mantisbt/print_bug_page.php?bug_id=2453)

[^f5]: [http://www.graphviz.org/content/cannot-install-graphviz-rhelrepo-error-404](http://www.graphviz.org/content/cannot-install-graphviz-rhelrepo-error-404)

[^f6]: [https://en.wikipedia.org/wiki/DOT_(graph_description_language)](https://en.wikipedia.org/wiki/DOT_(graph_description_language))

[^f7]: [http://www.graphviz.org/content/dot-language](http://www.graphviz.org/content/dot-language)

[^f8]: https://en.wikipedia.org/wiki/DOT_(graph_description_language)#Syntax