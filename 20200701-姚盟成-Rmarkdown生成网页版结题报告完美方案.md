---
title: Rmarkdown生成网页版结题报告完美方案
date: 2020-07-01 14:54
author: 尧小飞
top: false
cover: false
toc: true
summary:
tags: 
 - Rmarkdown
categories: 生产力工具

---


# 1.目的

之前写过一篇关于markdown的文章**typora+picgo+gitee组合的markdow神器**，介绍了如何优雅方便的使用markdown，如果你以为这就是markdown的全部功能，那你就大错特错了，markdown除了能够方便的记录笔记，还能很方便的生成pdf和html文件，便于文件的中间和查看，window下这个功能都很方便的实现，直接通过typora就可以实现，具体可以按照下图操作方法进行导出即可。

![](https://gitee.com/yao_xiao_fei2/figupload/raw/master/Rmarkdown生成网页版结题报告完美方案/20200701113409.png)

既然markdown可以这么方便的生成html和pdf文件，那么我们为何不不用它生成结题报告呢？目前我们的结题报告生成方式比较复杂，需要java和在其他特殊节点数生成，如果能够自己自动根据markdown生成结题报告就方便多了；另外一个问题是windows系统下一个文件可以按照上述的方式生成html文件，但是结题报告的话，肯定会很多，需要自动化，因此我们需要一个在linux系统下能够自动根据markdown文件生成html文件的方案。

# 2. 方案

根据markdown文件，在linux系统下生产html文件的方式有很多，比如pandoc，这是最常用的转换工具，经常通过pandoc将markdown文件转换成html格式的文件，然后通过wkhtmltopdf将html文件转换成pdf文件；虽然上述方式是常用的方法，但是我之前用的是另外一种方法进行转换成html格式，i5ting_toc，这也是一个比较简单的方法，不过这个需要在集群有root权限，但是由于现实条件，因此不推荐，i5ting_toc具体方案如下：

```shell
npm install -g i5ting_toc
i5ting_toc -f sample.md -o
```

最近无意中看到了rmarkdown的说明，发现很多R包的使用说明都是用的人rmarkdown进行示范html格式的撰写，出于好奇，然后我仔细查了一下，发现rmarkdown真是基于markdown生成html格式的神器，而且有很多相应的工具可以配合。

rmarkdown是一款R的包，这个包依赖pandoc等工具，但是rmarkdown包自己使用特别的方便，而且这个工具可以嵌套代码，然后运行代码，直接将代码结果展示在生成的html文件上，当然也可以仅仅展示代码；另外，这个工具不仅仅可以运行R代码，还能运行python代码，这简直是不能比这个在方便了。更为重要的是，rmarkdown工具的安装使用也特别的方便。rmarkdown的安装：

![](https://gitee.com/yao_xiao_fei2/figupload/raw/master/Rmarkdown生成网页版结题报告完美方案/20200701145348.png)

```R
install.packages('rmarkdown')
library(rmarkdown)
```

当然如果相对结果进行美化，可以安装一些其他的包，比如下面的包

```R
library(kableExtra)
library(dplyr)
library(DT)
library(dygraphs)
library(htmltools)
library(plotly)
```

上面介绍了rmarkdown所需要的基础的一些工具，rmarkdown是基于markdown的文件生成html文件的，因此我们首先需要一个markdown格式的文件，然后再利用R的rmarkdown进行格式转换，至于markdown的文件撰写，这里将不进行介绍，下面将对rmarkdown几个重要的和常见的内容进行介绍。

# 3. 设置 YAML

YAML是rmarkdown生成html最重要的参数之一，这里控制这全局参数、其他嵌套html文件、生成的输出文件格式等等。

```markdown
---
title: "Mardow Test" #"Some Title, `r format(Sys.time(), '%d %B, %Y')`"
author: "Mengcheng Yao"
output:
  html_document:
#    theme: sandstone #主题
#    highlight: tango #主题相关设置，增强可读性，有无数的高亮主题可选，
    toc: true #可对全文档添加目录
    toc_float:
        collapsed: false #可对全文档添加目录
    number_sections: true ##目录自动编号
---

```

其中最重要的绝对应该是toc，这个控制着是否生成左侧目录的关键参数，是否有编号等，正因为左侧目录，所以我才选择rmarkdown重新进行报告生成。

![](https://gitee.com/yao_xiao_fei2/figupload/raw/master/Rmarkdown生成网页版结题报告完美方案/20200701155658.png)

YAML全局参数如下（参考[Markdown学习与使用](https://www.jianshu.com/p/f50ac311b591)）：

```markdown
---
title: "Habits"
output: 
  html_document:
    toc: ture                  #是否展示目录
    toc_float: true             #目录的形式，是否浮动
    number_sections: true       #各个标题的数字标记是否展示
    df_print: paged           #表格的形式，paged创建可分页的表
    theme: cerulean              #文档主题，来源于Bootswatch(https://bootswatch.com/)
    highlight: tango               #指定语法高亮样式
    #css: css/styles.css       #加入额外的CSS，如果想从自己的css为文档提供所有样式，theme和highlight可设置为null
    #fig_width: 7                  #图片宽度
    #fig_height: 6                 #图片高度
    #fig_caption: TRUE       #图片设置，控制图形是否带有标题
    #code_folding: hide        #是否隐藏代码块
    #self_contained: false    #在外部文件中保留依赖关系
    #keep_md: true              #是否在knitr处理，pandoc渲染之后保存一份markdown文件的副本
    #template: quarterly_report.html   #可以使用模板选项替换基础pandoc模板
---
```

最终生成结题html文件方式，可以通过R交互界面即可：

```R
render("infercnv改造记录.Rmd", "html_document")
##或者
#R -e "rmarkdown::render('infercnv改造记录.Rmd',output_file='output.html')"
```

**注：这里一定要注意文件的名称，开始测试的时候用的文件为：infercnv改造记录.Rmd，结果文件中的R代码死活不执行，折腾了一个上午，都没有解决，后面无意中改了一个名称就可以了，因此文件的名称一定要以rmd结尾。**

# 4. 图片

我们做任何笔记或者介绍的时候，图片绝对是主力，没有图片的介绍和笔记是没有灵魂的，图片能够加深我们对笔记和内容的理解，图片是最常见的展示方式。这里的图片展示，有常见的markdown的文件展示方式，这里不用多介绍，下面给几个例子即可：

```markdown
![Infercnv结果图](typora-user-images/image-20200402105122226.png) ##本地图片
![img](https://upload-images.jianshu.io/upload_images/9376801-74741cf656de39a4.png?imageMogr2/auto-orient/strip|imageView2/2/w/762/format/webp) #网页图片
```

常见的图片基本上满足markdown的需求，但是rmarkdown的另外一个优势是，不仅仅能够展示markdown的图片，而且还能直接运行代码块中的代码，展示实时生成的图片：

~~~markdown
```{r}
example(plot)  #添加任何r画图代码即可，或者展示数据
```
~~~

上面的例子仅仅展示了部分示范的图，最终生成的html格式的文件，会得到如下的图片，结果会发现把图片直接展示出来了，这就是他最大的几个优势之一。

![](https://gitee.com/yao_xiao_fei2/figupload/raw/master/Rmarkdown生成网页版结题报告完美方案/20200701150430.png)

这样的话，这个rmarkdown的应用场景就可以大大的增加了，一是：我们可以将代码和和结果都展示出来，并与对R代码的理解；二是：可以根据任何项目中某个类型的数据，实时的展示这个项目的数据，这将是十分方便的。

# 5. 动态图

一般来说，如果想看动态结果图，需要python的*Plotly*进行展示，这里需要另外画图打开html文件，不是很方便，但这对应rmarkdown完全不成问题，上面提到了，rmarkdown可以展示R代码的运行结果，R语言中不是有plot_ly可以画动态图嘛，结合其他，不就可以直接展示动态图，直接在结题报告中展示相关的分析内容。

~~~markdown
```{r}
library(plotly)
plot_ly(data = iris, x =~Sepal.Length, y =~Petal.Length, mode = "markers",color =~Species)
```
~~~

![](https://gitee.com/yao_xiao_fei2/figupload/raw/master/Rmarkdown生成网页版结题报告完美方案/20200701153554.png)

运行上面 的代码，发现动态图直接整合在html文件中，而且具有多有动态图结果的功能，比如数据筛选、选择、放大、鼠标放在数据上可以展示具体点的信息等。

# 6. 表格

表格是笔记内容展示的另外一种形式，当然，这里可以展示markdown的表格，比如如下表格：

![](https://gitee.com/yao_xiao_fei2/figupload/raw/master/Rmarkdown生成网页版结题报告完美方案/20200701151142.png)

rmarkdown的表格展示，一般是通过knitr展示，原始的表格可能有时候会比较乱，但是通过knitr的展示，就比较整齐。

```R
library(knitr)
kable(data)
```

![](https://gitee.com/yao_xiao_fei2/figupload/raw/master/Rmarkdown生成网页版结题报告完美方案/20200701151326.png)

除了基础的表格展示，rmarkdown还可以配合kableExtra进行对表格美化，比如对背景颜色等进行修改，可以得到如下表格：

```R
library(kableExtra)
library(dplyr)
iris[1:10,] %>%  kable() %>%  kable_styling("striped", full_width = F) %>%   column_spec(2:4, bold = T) %>%  row_spec(3:5, bold = T, color = "white", background = "#D7261E")
```

![](https://gitee.com/yao_xiao_fei2/figupload/raw/master/Rmarkdown生成网页版结题报告完美方案/20200701151501.png)

美化过的表格对2-5行进行背景高亮红色，突出重点，当然此函数还有其他的一些功能，如果自己感兴趣的话，可以试一试。

# 7. 动态筛选表格

rmarkdown另外一个比较好的的功能可以生成动态筛选表格，这个在数据筛选方面具将十分方便：

~~~markdown
```{r}
library(DT)
DT::datatable(iris,filter="top",options = list(pageLength = 10, scrollX=T))
```
~~~

![](https://gitee.com/yao_xiao_fei2/figupload/raw/master/Rmarkdown生成网页版结题报告完美方案/20200701154954.png)

这里生成的动态表格十分的方便而且功能强大，可以对整个表格进行字段筛选或者每一列的字段进行筛选，每一页展示的行数也可以调整，这是最快的生成动态筛选表格的方法。

# 8. 代码

rmarkdown与普通markdown最大的不同，个人认为是其对代码的支持不同，上面介绍了代码画图，这是他与其他markdown最大的不同，可以根据一定的规则执行代码，并且将代码执行的结果展示在结果中。

~~~markdown
```{r}
library(kableExtra)
library(dplyr)
iris[1:10,] %>%  kable() %>%  kable_styling("striped", full_width = F) %>%   column_spec(2:4, bold = T) %>%  row_spec(3:5, bold = T, color = "white", background = "#D7261E")
```
~~~

代码与代码之间有···符号相连接，中间就是r代码，这里可以是任何r代码，而且为了控制r代码的执行结果展示，这里可以在{r 参数=参数}这里添加很多参数，具体的参数如下（[RMarkdown的基本设置及语法](https://www.jianshu.com/p/f71fac797a6c)）：

~~~php
```{r setup, include=FALSE}
knitr::opts_chunk$set(eval=TRUE, #在块中运行代码(default = TRUE)
  tidy=FALSE, #是否整理代码
  prompt=TRUE, #代码是否添加引导符’>‘
  highlight=TRUE, #高亮显示源码 (default = TRUE)
  comment="Result:", #结果的每一行加前缀(default = ‘##’)
  echo=TRUE, #是否在输出中包含源代码
  results="markup", #装裱markup、原样asis、隐藏hide、末尾hold
  collapse=FALSE,#把所有的输出汇聚到单个块中(default = FALSE)
  warning=FALSE,#是否在输出中包含警告(default = TRUE)
  error=TRUE, #是否在输出中包含错误信息
  message=FALSE, #是否在输出中包含参考的信息
  split=FALSE, #是否将R的输出分割成单独的文件
  include=TRUE, #运行后是否在文档中显示块 (default = TRUE)
  cache = TRUE, #对代码段打开缓存
  cache.path="./cache_file/", #缓存结果的保存路径 (default = “cache/”)
  # fig.path="figure/prefix-", #图片路径，支持非前缀模式(‘figure/’)
  # fig.keep="high", #保存图形类型，高级high、不保存none’)、所有图形(‘all’)、第一张(‘first’)、最后一张(‘last’)
  fig.show="asis", #展示方式，紧随代码asis、最后统一hold、动画animate
  # fig.width=8, #可以用%表示
  # fig.height=6, #图片文件的宽、高(英寸2.54cm 为单位)
  # out.width=8,
  # out.height=6, #图片在输出文档中的宽、高
  fig.align="center" #对齐方式，不做调节(‘default’)、左(‘left’)、右(‘right’)、居中(‘center’)
  # interval=1 #动画参数，切换画面时间，单位为秒
)
~~~

除了支持R代码，rmarkdown也支持python代码：

~~~markdown
```{python}
print("yaomengcheng")
```
~~~

同样也可以运行代码并输出结果。

![](https://gitee.com/yao_xiao_fei2/figupload/raw/master/Rmarkdown生成网页版结题报告完美方案/20200701153135.png)

# 9. 嵌套本地html跳转链接

除了正常的图片展示以为，有时候还有很多外来的html需要展示或者链接，其实这里的链接就是markdown为链接方式一致，比如本地有一个web_summary.html文件，那么如果想要直接点击这个文件就可以按照如下方式进行：

```markdown
[web_summary测试](web_summary.html) #
```

![](https://gitee.com/yao_xiao_fei2/figupload/raw/master/Rmarkdown生成网页版结题报告完美方案/20200701154731.png)

直接点击链接，即可直接打开改web_summary.html。

# 10. 总结

总的来说，到目前为止，rmarkdown是自动化生成html和pdf报告的最优方式，它不仅使用方便，而且功能强大，兼容性较好，因此今后所有的小的结果总结或者报告都可以通过此方式生成结题报告。



2020年7月1日 