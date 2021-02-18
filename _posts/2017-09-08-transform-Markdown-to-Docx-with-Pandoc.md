---
layout: post
title: "用Pandoc转化Markdown到Docx"
---

## 问题背景

文本写作的最大的好处是，文字可以像代码一样进行版本化的管理，特别是结合当下GitHub的Pages功能，尝试之后简直让人爱不释手。本人也是“中毒”颇深，在新书《Python编程基础与HTTP接口测试》的编写过程中一直采用Markdown进行编写，很是顺畅。

但是，由于Markdown的相关工具均为国外的开源工具，使用过程中难免需要一些探索，特别是用来写书，相对于写博客和文章来说，更是需要一点点使用技巧，国内的审校、编辑和交付出版过程中，很多出版社都是采用Word的Docx格式来作为内容的载体进行交互。

本文就我写书过程中遇到的几个问题进行探索和解决：

* Markdown如何顺畅的转换为Docx格式的问题？
* 转换的过程中如何进行各种不同内容的样式定义问题？

## 解决方案

### Markdown如何顺畅的转换为Docx格式的问题

这个问题其实比较好解决，解决的方案就是Pandoc。关于Pandoc的下载，安装就不多说了，文章一大堆。

总结起来就是，下载并安装之后，运行一条命令。搞定！

命令的示例如下：

    pandoc -f markdown -t docx  mybook.md -o mybook.docx

这条命令，就是将一个markdown格式的文本mybook.md转为为一个docx格式的word文件mybook.docx。


### 转换的过程中如何进行各种不同内容的样式定义问题？

在中文的写作过程中，每个段落首行一般都会缩进两个汉字，而我们通过pandoc默认转换出来的docx格式的文档中是没有进行首行缩进的，因为英文没有这个书写习惯。同时，在转换后我还发现，我书中的代码段，在被转换后并没有像转换到HTML的时候一样被放到带阴影的大框里，而是和普通的正文一样白底黑字。

转换过程中如何进行各种不同的内容的样式的自主定义就成为必须解决的一个问题。针对这个问题我查了不少资料，多说的只言片语，不完整，这里讲我经过多方尝试，后的一个比较完整的解法，详细的记录下来以备新学。大致的思路是使用自定义的转换模板(reference.docx)。

具体的步骤如下：

**首先，生成一个基准的转换模板。**

得到基准模板(reference.docx)的命令如下：

    pandoc --print-default-data-file reference.docx > custom-reference.docx

运行后，你得会得到一个名字为custom-reference.docx的word文件，文件中只有一个"Hello world"。其实，重要的不是这个文件中的内容，是这个文件里的Word样式(Style)。很多文章中都说要对这个文件中的样式进行修改，但是，对于如何修改都基本上没有什么说明。


**第二，客户化得到的转换模板。**

生成基准模板文件(custom-reference.docx)之后，就是对该基准文件中对应的样式进行修改。
以我写书过程中遇到的两个问题为例：

1、段落首行缩进两个汉字：

在中文的书写过程中，每个段落首行一般都要求缩进两个汉字，这就需要修改 custom-reference.docx 文件中的“First Paragraph”和“文本正文”两个样式，将样式的首行缩进设置为两个字符。

2、重新定义代码段样式：

我写的书中有不少代码段，这些代码段经过pandoc转换后，并没有进行特殊的样式处理。代码段在Markdown中是通过每行缩进4个字节进行定义的，而在Word中代码段对应的样式是“Source Code”样式。

值得注意的是，通过上面的方法生成的基准模板（custom-reference.docx）中并没有“Source Code”样式，所以，读者需要自行添加该样式，然后修改“Source Code”样式，增加边框设置，并将背景设置为浅灰色，这样代码块在Word文档中的展示效果就会与在HTML中一样了。


**最后，基于转换模板运行pandoc命令，生成Word文件。**

其命令比上面的小节的转换命令略微复杂一点。

    pandoc -f markdown -t docx --reference-docx=custom-reference.docx mybook.md -o mybook.docx 


**一点小提示：**

Word的“开始”页面中显示的样式并不是该Docx文件中定义的所有样式，要查看Docx文件中定义的所有样式需要点击“样式”工具栏右下角的**小箭头**，点击后会出现样式视图，在样式视图中会显示文件中定义的所有样式，并且可以对选定的样式进行修改，以及新增样式。也可以通过快捷键（Alt+Ctrl+Shift+S）打开样式视图。

另外，通过上面的命令行生成的基准模板（custom-reference.docx）中有很多样式不存在，比如上面提到的“Source Code”样式，需要读者自行手工添加才行。我推荐通过下面更为直接的方式获取基准模板

* 首先建立一个Hello.txt文件，其中只有一行文字“Hello World”，
* 然后运行如下代码：  
    
    pandoc -f markdown -t docx  hello.txt -o custom-reference2.docx

这样获取的基准样式模板中的所有样式都是已经定义的了，读者根据自己的需要调整对应的样式即可，而不再需要新添加样式。

## 参考链接	

http://pandoc.org/

http://pandoc.org/scripting.html

http://pandoc.org/MANUAL.html

http://pandoc.org/MANUAL.html#custom-styles-in-docx-output

https://stackoverflow.com/questions/28346388/changing-the-pandoc-monospace-font-size-or-style-in-docx-output