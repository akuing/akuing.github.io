---
layout: post
title: "用Pandoc转化Markdown到Docx"
---

## 问题背景

文本写作的最大的好处是，文字的版本化管理，特别是结合当下GitHub的Pages功能，尝试之后简直让人爱不释手。本人也是“中毒”颇深，在新书《Python编程基础与HTTP接口测试》的编写过程中一直采用Markdown进行编写，很是顺畅。

问题是国内的审校、编辑和交付出版过程中，很多出版社还是采用Word的Docx格式。

* Markdown如何顺畅的转换为Docx格式？
* 转换的过程中如何进行各种格式的定义？比如，我书中的代码段，在被转换后并没有像转换到HTML的时候一样被放到带阴影的大框里，而是白底黑字和正文的样式没有什么区别。

## 解决方案

### 格式转换的问题比较好解决

解决的方案就是Pandoc。关于Pandoc的下载，安装就不多说了，文章一大堆。

### 转换过程中如何进行格式定义？

转换过程中的格式定义的确是个问题，查了不少资料，都说的比较粗浅，大部分都是提到reference.docx，然后就没有然后了。

Markdown转换为Docx过程中，为了可以进行自主的格式定义，首先需要进行转换过程中的参考模板**reference.docx**的客户化。

要客户化模板，首先就要得到基准模板。

产生一个客户化的基准reference.docx的命令如下：

    pandoc --print-default-data-file reference.docx > custom-reference.docx

运行后，你得到的 custom-reference.docx 文件中只有一个"Hello world"。其实，重要的不是这个文件中的内容，是这个文件里的样式(Style)。

在中文的书写过程中，每个段落首行都会缩进两个汉字，需要修改 custom-reference.docx 文件中的“First Paragraph”和“文本正文”两个样式，将样式的首行缩进设置为两个字符。

我写的书中有不少代码段，这些代码段经过pandoc转换后，并没有进行特殊的样式处理。代码段在Markdown中是通过每行缩进4个字节进行定义的，在Word中对应的是“Source Code”样式，通过上面的方法生成的基准模板（custom-reference.docx）中是没有“Source Code”样式的，需要自行添加该样式，然后根据自己的需要修改“Source Code”样式，比如设置边框，并将背景设置为浅灰色，这样代码块在Word文档中的展示效果就与在HTML中一样了。

**一点小提示：**

Word的“开始”页面中显示的样式并不是该Docx文件中定义的所有样式，要查看Docx文件中定义的所有样式需要点击“样式”工具栏右下角的**小箭头**，点击后会出现样式视图，在样式视图中会显示文件中定义的所有样式，并且可以对选定的样式进行修改，以及新增样式。也可以通过快捷键（Alt+Ctrl+Shift+S）打开样式视图。

通过上面的命令行生成的基准模板（custom-reference.docx）中有很多样式不存在，比如“Source Code”样式，需要读者自行手工添加才行。我推荐通过下面的方式更为直接的方式获取基准模板*

* 首先建立一个Hello.txt文件，其中只有一行文字“Hello World”，
* 然后运行如下代码：  
    
    pandoc -f markdown -t docx  hello.txt -o custom-reference2.docx

这样获取的基准样式模板中的所有样式都是已经定义的了，读者根据自己的需要调整对应的样式即可，而不再需要新建样式。

## 参考链接	

http://pandoc.org/
http://pandoc.org/scripting.html
http://pandoc.org/MANUAL.html
http://pandoc.org/MANUAL.html#custom-styles-in-docx-output
https://stackoverflow.com/questions/28346388/changing-the-pandoc-monospace-font-size-or-style-in-docx-output