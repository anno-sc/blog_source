---
title: cellranger4.0抢先看
date: 2020-07-12 14:36
author: 尧小飞
top: false
cover: false
toc: true
summary:
tags: 
 - cellranger
categories: QC

---

# 1. 背景介绍

做单细胞转录组的都知道，目前为止[10xGenomics公司](https://www.10xgenomics.com/)的单细胞转录组解决方案是单细胞领域中的绝对领导者，预计基本上占单细胞转录组的市场份额80%以上（或者更高），因此10xGenomics公司的任何风吹草动都会对单细胞研究的领域产生十分重要的影响。

![](https://gitee.com/yao_xiao_fei2/figupload/raw/master/cellranger4.0抢先看/20200712214446.png)

2020年7月初，10xGenomics公司推出了[靶向基因的解决方案](https://www.10xgenomics.com/products/targeted-gene-expression/)，号称在同等测序深度的条件下，其成本降低了90%以上，或者说起通量增加10倍，这对于老师所关注的领域的研究十分重要。今天主要介绍的是cellranger4.0，为什么要所靶向基因呢？主要是这里推出了新产品靶向基因，肯定得有分析软件，而靶向基因使用的软件正好是cellranger4.0，cellranger4.0正好进行了更新，增加了新的的功能和提高了效率。

![](https://gitee.com/yao_xiao_fei2/figupload/raw/master/cellranger4.0抢先看/20200712214949.png)

# 2. cellranger4.0更新介绍

cellranger4.0更新日期是7月7日，我查看了一下[Release notes for Cell Ranger 4.0 (07/07/2020)](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/release-notes)，一共一千多字，介绍了很多更新的内容和注意事项，大家可以自己去仔细的看看，我自己大概总结了一下，主要有以下几个方面：

> 1. Targeted Gene Expression analysis。
> 2. Recommended reference packages for human and mouse have been updated from version 3.0.0 to 2020A。
> 3. Trimming for alignment。
> 4. BAM file changes。
> 5. **count and vdj run approximately two to four times faster than in Cell Ranger 3.1, depending on the sequencing data, and reduces disk I/O by half**。
> 6. Recommended VDJ reference packages for human and mouse have been updated from。

当然还有别的一些内容，个人觉得上面的节点重要，就不介绍了。其实我们关注的重点在于，由于靶向基因解决方案还没有正式推出，这里不介绍，**总结到两个方面：提高了比对效率和提高了比对率。其中比对效率提高了差不多2-4倍，比对率大概能提高1.5%左右**。这两方面对于我们目前单细胞转录组分析来说，其帮助是十分巨大的，特别是公司，提高效率2-4倍，以后就可以减少机时，节约成本，缩短交付周期，而且同时其比对率还有相应的提高，这真是太实用了。上面是软件自我介绍，具体如何，下面将对其进行测试。

# 3. cellranger4.0 测试

这里使用了一个人的测试数据，分别进行了三组测试：cellranger-3.1.0（之前的参考基因组）和cellranger-4.0.0（新的的参考基因组和之前的参考基因组）。测试命令如下：

```shell
#A
cellranger-3.1.0/cellranger count --id=  --fastqs= --sample=  --transcriptome=cellranger-GRCh38-3.0.0 --localmem=40 --localcores=8
#B
cellranger-4.0.0/bin/cellranger count --id=  --fastqs= --sample=  --transcriptome=cellranger-GRCh38-3.0.0 --localmem=40 --localcores=8
#C
cellranger4.0/cellranger-4.0.0/bin/cellranger count --id=  --fastqs=  --sample=  --transcriptome= refdata-gex-GRCh38-2020-A/ --localmem=40 --localcores=8
```



|      条件      |               A               |               B               |               C               |
| :------------: | :---------------------------: | :---------------------------: | :---------------------------: |
|    软件版本    |       cellranger-3.1.0        |       cellranger-4.0.0        |       cellranger-4.0.0        |
|   参考基因组   |         GRCh38-3.0.0          |         GRCh38-3.0.0          |         GRCh38-2020-A         |
|      参数      | --localmem=40  --localcores=8 | --localmem=40  --localcores=8 | --localmem=40  --localcores=8 |
|    开始时间    |      2020-04-13 10:16:52      |      2020-07-10 17:00:50      |      2020-07-10 17:16:33      |
|    结束时间    |      2020-04-14 06:08:20      |      2020-07-11 04:30:57      |      2020-07-11 00:27:44      |
|      耗时      |              20h              |             11.5h             |              7h               |
|    最大内存    |            30.499G            |            35.270G            |            35.281G            |
|       IO       |           14497.833           |           5555.653            |           5592.511            |
|    计算节点    |          c0803.cloud          |          c0002.local          |          c0018.local          |
|   预测细胞数   |             1071              |             1079              |             1086              |
|   基因中位数   |              351              |              354              |              361              |
|     比对率     |             0.926             |             0.913             |             0.913             |
|  转录本比对率  |             0.439             |             0.444             |             0.454             |
| 总基因检出数目 |             19316             |             19420             |             20989             |

这里三个比较组，cellranger-3.1.0之前已经运行过一次，因此这次不再运行，从上表格可以看出，测试结果基本上达到了cellranger4.0的Release notes的结果，如果按照相同的参考基因组，都适用GRCh38-3.0.0参考基因组的话，cellranger-3.1.0耗时为20h左右，cellranger-4.0.0为11.5h左右，差不多两倍，如果换成GRCh38-2020-A参考基因组，耗时只有7h左右，差不多是3倍左右，因此比对效率基本上达到要求。cellranger4.0的另外一项是比对率提高，上述表格表明，其细胞的基因检出中位值有一定的提高，特别是用新的参考基因组以后，提高了10左右，比对率基本上没有什么变化，但是有效转录本比对率有一定的提高，在新的参考基因组条件下，比对率差不多提高了1.5%左右，因此基本上达到要求。

另外需要注意的是，这次测试是用的qsub进行投递，因此计算节点不是一个节点，这可能会造成效率有些微波动，不过对结果影响不会太大；cellranger4.0虽然速度提高了，但是内存也增加了，差不多增加了17%左右，这可能是代价，用新的参考基因组以后，其基因检出总数增加了1400左右。

# 4. 总结

整体来说，这次cellranger4.0更新的作用还是挺大的，特别是对于公司来说，其速度差不多增加了2倍左右，这大大的提高了效率；另外一方面，其比对率的确有所增加，差不多1.5%左右，这是在提高效率的同时，没有牺牲准确性，相反还提高了结果的准确性；不过需要注意的是，cellranger4.0的最大内存增加了，这可能就是提高效率的代价，不过17%的内存增加换来效率的翻倍，这是值得的。

新版的cellranger4.0发布的同时，其参考基因组也有所有更新，而且对有一定的影响，因此建议更新软件的同时，也要更新参考基因组。

上面主要介绍了cellranger count的命令，也就是主要是cellranger对转录组的分析，对于VDJ分析，其测试结果基本上类似，我用了小数据量测试，基本上也是节省1倍的时间。





2020年7月12日