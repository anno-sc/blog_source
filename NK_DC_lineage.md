---
title: DC和NK细胞的lineage及marker基因
date: 2020-09-25 16:00
author: Spartanzhao
top: false
cover: false
toc: true
summary:
tags: 
 - DC
 - NK
categories: 免疫细胞

---

> 在分析单细胞RNA的时候，最为重要的就是细胞命名。细胞聚类最重要的方法之一，就是利用marker基因在不同细胞中的表达差异，来对细胞命名。本文就介绍`NK`细胞核`DC`细胞的lineage以及相应的cell marker

## NK细胞

NK细胞的lineage:

![transcription_bursting](https://gitee.com/anno-sc/blog_source/raw/master/figure/DC_marker/common_lymphoid_progenitors.png)


NK 细胞由common lymphoid progenitors分化而来，经过多次分化后，形成NK细胞，ILC1， ILC2， ILC3, 以及LTi细胞，这些细胞相应的功能与cell marker:

![sc.bias](https://gitee.com/anno-sc/blog_source/raw/master/figure/DC_marker/Innate_lymphoid_cells.png)

除此之外，还存在另外一类细胞，叫`NKT细胞`，这一类细胞既有NK细胞的特征，也有T细胞的特征，但是一般把这一类细胞归类为T细胞。相关marerk为：

![bias](https://gitee.com/anno-sc/blog_source/raw/master/figure/DC_marker/NKT.png)

## DC细胞
DC细胞相对而言，比NK细胞要复杂的多：

![bias](https://gitee.com/anno-sc/blog_source/raw/master/figure/DC_marker/DC_lineage1.png)

DC细胞一般是从骨髓中的HSC发育而来，更详细的lineage极其功能如下图所示：

![bias](https://gitee.com/anno-sc/blog_source/raw/master/figure/DC_marker/DC_lineage2.png)

DC主要分为pDC, cDC1, cDC2,和mo-DC, 其中mo-DC是从单核细胞发育而来，而其他cDC1, cDC2是从pre-DC发育而来， pDC从CDP发育而来：如图所示：

![bias](https://gitee.com/anno-sc/blog_source/raw/master/figure/DC_marker/DC_lineage3.png)

DC细胞的发育时非常复杂的，来源也很复杂，多种DC的前体细胞可以发育成同一种DC细胞，而多种DC细胞也可以是从同一种前体细胞中发育而来：

![bias](https://gitee.com/anno-sc/blog_source/raw/master/figure/DC_marker/DC_lineage4.png)

关于DC的cell marker更加复杂：同一种DC细胞，但是在不同的组织中，其cell marker都有可能不一样：

![bias](https://gitee.com/anno-sc/blog_source/raw/master/figure/DC_marker/Dendritic_cells.png)



在这里，我列出一些主要的DC细胞类型的cell marker:

![bias](https://gitee.com/anno-sc/blog_source/raw/master/figure/DC_marker/DCs.png)

目前这就是我总结的全部关于NK细胞以及DC细胞相关的内容。

