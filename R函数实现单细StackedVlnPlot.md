---
title: R函数实现单细胞StackedVlnPlot
date: 2020-08-04 13:39
author: ahworld07
top: false
cover: false
toc: true
summary: 
tags: 
 - singlecell
 - VlnPlot
categories: plot

---

![](https://gitee.com/anno-sc/blog_source/raw/master/figure/StackedVlnPlot/svln_01.png)

上图这样的单细胞`StackedVlnPlot`在高分文章中出现比较多，比较适合美观的展示多个marker gene的表达分布，而目前`Seurat`画小提琴图的函数`VlnPlot`是不能实现这样堆叠效果的。

我们在《[seurat结果转scanpy画StackedVlnPlot](https://mp.weixin.qq.com/s/BTQqdt7mtZx8t7mi9xhpAw)》中介绍过`scanpy`中的`sc.pl.stacked_violin`函数可以实现**StackedVlnPlot**的功能：


![](https://gitee.com/anno-sc/blog_source/raw/master/figure/StackedVlnPlot/svln_02.png)


我们在《[生信工程师的自我修养](https://mp.weixin.qq.com/s/drfZP06ixuTjtVteQf-GHA)》中介绍过怎样修改`scanpy`的`sc.pl.stacked_violin`实现小提琴图横放堆叠的效果：

![](https://gitee.com/anno-sc/blog_source/raw/master/figure/StackedVlnPlot/svln_03.png)


`Seurat`结果转`loom`格式让很大一部分人望而却步，选择了放弃画StackedVlnPlot，或者宁愿用AI实现。

所以，今天分享给大家一个**用R原生函数实现StackedVlnPlot的方法**，需要用到`patchwork`包，代码如下：

```R
library(Seurat)
library(ggplot2)

modify_vlnplot<- function(obj, 
                          feature, 
                          pt.size = 0, 
                          plot.margin = unit(c(-0.75, 0, -0.75, 0), "cm"),
                          ...) {
  p<- VlnPlot(obj, features = feature, pt.size = pt.size, ... )  + 
    xlab("") + ylab(feature) + ggtitle("") + 
    theme(legend.position = "none", 
          axis.text.x = element_blank(), 
          axis.text.y = element_blank(), 
          axis.ticks.x = element_blank(), 
          axis.ticks.y = element_line(),
          axis.title.y = element_text(size = rel(1), angle = 0, vjust = 0.5),
          plot.margin = plot.margin )
  return(p)
}

## main function
StackedVlnPlot<- function(obj, features,
                          pt.size = 0,
                          plot.margin = unit(c(-0.75, 0, -0.75, 0), "cm"),
                          ...) {
  
  plot_list<- purrr::map(features, function(x) modify_vlnplot(obj = obj,feature = x, ...))
  plot_list[[length(plot_list)]]<- plot_list[[length(plot_list)]] +
    theme(axis.text.x=element_text(), axis.ticks.x = element_line())
  
  p<- patchwork::wrap_plots(plotlist = plot_list, ncol = 1)
  return(p)
}
```

这个`StackedVlnPlot`是对**Seurat**的**VlnPlot**方法的封装，使用方法同VlnPlot，输入数据是Seurat对象，让我们看下效果：

```R
StackedVlnPlot(sdata, c('Retnlg', 'Pygl', 'Anxa1', 'Igf1r', 'Stfa2l1'), pt.size=0, cols=my36colors)
```

![](https://gitee.com/anno-sc/blog_source/raw/master/figure/StackedVlnPlot/svln_04.png)


如果需要对图做细微的调整，在了解**ggplot2**的基础上，可以尝试对上面的函数进行定制修改。

配色方案也一并分享：

```R
my36colors <-c('#E5D2DD', '#53A85F', '#F1BB72', '#F3B1A0', '#D6E7A3', '#57C3F3', '#476D87',
               '#E95C59', '#E59CC4', '#AB3282', '#23452F', '#BD956A', '#8C549C', '#585658',
               '#9FA3A8', '#E0D4CA', '#5F3D69', '#C5DEBA', '#58A4C3', '#E4C755', '#F7F398',
               '#AA9A59', '#E63863', '#E39A35', '#C1E6F3', '#6778AE', '#91D0BE', '#B53E2B',
               '#712820', '#DCC1DD', '#CCE0F5',  '#CCC9E6', '#625D9E', '#68A180', '#3A6963',
               '#968175'
)
```

