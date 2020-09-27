---
title: T细胞VDJ的TCR_dist分析结果说明
date: 2020-09-27 14:54
author: 尧小飞
top: false
cover: false
toc: true
summary:
tags: 
 - T细胞
 - VDJ
categories: VDJ

---

# 背景说明

T细胞由异源二聚体表面受体T细胞受体（TCR）定义，该受体通过与肽和主要组织相容性复合物（pMHCs）相互作用介导病原体相关表位的识别。 TCR是通过种系TCR基因座的基因组重排产生的，该过程称为V（D）J重组，具有产生显着TCR多样性的潜力（估计范围为10 的15次方至10的61次方）可能的受体）。尽管存在这种潜在的多样性，但来自识别相同pMHC表位的T细胞的TCR通常共享保守的序列特征，这表明可以预测性地模拟表位特异性。

TCR_dist是来自美国圣犹大儿童研究医院和弗雷德哈钦森癌症研究中心等研究机构的研究人员于2017年开发出一种功能类似于罗塞塔石板（Rosetta Stone）的算法，该算法有助破解免疫系统如何识别和结合抗原。它应当有助开发更加个人化的癌症免疫疗法，并且有助加快诊断和治疗传染病。

安诺利用TCR_dist工具，对10xGenomics单细胞的VDJ进行分析，从而研究其TCR的序列特征以及TCR基因的分布特征，从而获取其发挥主要作用的VDJ序列和VDJ基因。

# 分析流程

![](https://gitee.com/yao_xiao_fei2/figupload/raw/master/安诺T细胞VDJ的TCR_dist分析结果说明/20200927144932.png)

# 主要结果说明

TCR_dist工具的结果较多，结果也比较深入，但是目前应用常见的结果VDJ分布的序列结果和VJ基因的特征分布。具体结果可以见结果目录下的样品名称为名的文件的index.html文件，结果说明都在这个html文件中。

[1] summary table，这里主要是整个结果统计：

![](https://gitee.com/yao_xiao_fei2/figupload/raw/master/安诺T细胞VDJ的TCR_dist分析结果说明/20200927145310.png)

[2] CDR3统计结果

![](https://gitee.com/yao_xiao_fei2/figupload/raw/master/安诺T细胞VDJ的TCR_dist分析结果说明/20200927145402.png)

[3]  TCR Clustering trees

下面的进化树状图，为A链中，所有的序列的进化树图，显示了不同CDR3序列在聚类上的分布。

![](https://gitee.com/yao_xiao_fei2/figupload/raw/master/安诺T细胞VDJ的TCR_dist分析结果说明/20200927145438.png)

[4]  kpca

下图为kpca降维图，展示了在整个样品中A和B链的主要的基因，下图从左到右分布为A链的V/J基因、B链的V/J基因，图中会展示最主要的基因名称，其中颜色的排列为红色表示频数最高，绿色（第二最频繁），蓝色，青色，品红色和黑色。

![](https://gitee.com/yao_xiao_fei2/figupload/raw/master/安诺T细胞VDJ的TCR_dist分析结果说明/20200927145513.png)

[5]  gene_segment_pies

![](https://gitee.com/yao_xiao_fei2/figupload/raw/master/安诺T细胞VDJ的TCR_dist分析结果说明/20200927145547.png)

[6]  基因分布长度图

由于VDJ基因的长度可能也会对抗原的结合产生一定的影响，因此对A/B链基因长度进行了统计，统计结果如下所示：

![](https://gitee.com/yao_xiao_fei2/figupload/raw/master/安诺T细胞VDJ的TCR_dist分析结果说明/20200927145611.png)

[7] VJ基因配对特征分析结果

下图反应了整个链中VJ基因配对结果，其中红色表示频率最高，绿色次之，四个基因的频数都是从上到下的减少，颜色都是从红色最高，绿色（第二最频繁），蓝色，青色，品红色和黑色。

![](https://gitee.com/yao_xiao_fei2/figupload/raw/master/安诺T细胞VDJ的TCR_dist分析结果说明/20200927145700.png)

[8] CDR3序列特征分析

下图为A/B链的特征分析结果图，这里A/B两条链的含义从左到右分布为：数字为频数，V基因名称明天频数，序列频数，J基因名称频数，聚类树状图，这里每一个先为一个clonetype，上面有虚线的才在左边的图进行展示，这里的数字与左边的数字对应。

![](https://gitee.com/yao_xiao_fei2/figupload/raw/master/安诺T细胞VDJ的TCR_dist分析结果说明/20200927145752.png)

#  适用范围

目前主要支持人和小鼠两个物种，暂时不支持其他物种，其实10xGenomics单细胞VDJ目前也主要是针对人和小鼠，不过目前分析只支持TCR分析，还不支持BCR，因此这个需要注意。

其他注意事项：**其实该工具分析主要针对抗原表位的，一个抗原表位一个类似样品名称，但是为了分析，这里用的是样品名称代替了抗原表位名称；另外，此分析结果是直接利用的10xGenomics单细胞VDJ结果中的CDR3序列，然后将序列与T细胞VDJ基因数据库进行blast，因此此结果不一定完全与10xGenomics单细胞VDJ的结果完全一致。**

# 参考文献

Dash P, Fioregartland A, Hertz T, et al. Quantifiable predictive features define epitope-specific T cell receptor repertoires[J]. Nature, 2017, 547(7661): 89-93.