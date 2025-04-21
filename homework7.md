# 2.1
# 1
常见的归一化方法

| 方法 | 描述 | 考虑因素 | 使用场景 | 公式 |
| :---: | :---: | :---: | :---: | :---: |
| **RPM or CPM**(reads/counts per million mapped reads) | 按照reads总数缩放计数 | 测序深度 | 同一样本组重复之间的基因计数比较；不适用于样本内比较或差异表达分析 | $RPMorCPM = \frac{Reads Number of a Gene \times 10^6}{Total Number of Mapped Reads}$ |
| **TPM**(transcripts per kilobase million) | 每百万读取reads比对的转录本长度(kb)计数 | 测序深度与基因长度 | 样本内或同一样本组样本之间的基因计数比较；不适用于差异表达分析 | $TPM = \frac{RPKM}{\sum RPKM} \times 10^6$ |
| **RPKM/FPKM**(reads/fragments per kilobase per million reads/fragments mapped | 类似于TPM | 测序深度与基因长度 | 同一样本组重复之间的基因计数比较；不适用于样本内比较或差异表达分析 | $RPKMorFPKM = \frac{Reads/Frgaments Number of Gene \times 10^3 \times 10^6}{Total Number of Mapped Reads \times Gene Length in bp}$ |
| **DESeq2's median of ratios** | 计数除以特定于样本的大小因子，该因子由基因计数相对于每个基因的几何平均值的中位数比率确定 | 测序深度和RNA组成 | 同一样本组重复之间的基因计数比较；不适用于样本内比较或差异表达分析 | 参考下面博客内内容 |
| **EdgeR's trimmed mean of M values(TMM)** | 使用样本之间对数表达比率的加权修剪平均值 | 测序深度和RNA组成 | 样品之间的基因计数比较和差异表达分析，不适用样本内比较 | 公式较复杂，可参考下面博客内内容 |


很好的总结博客：https://blog.csdn.net/weixin_46128755/article/details/126283762


# 2
E
D
A

# 3
```bash
root@featurecount_docker:/home/test# /usr/local/bin/infer_experiment.py -r GTF/Arabidopsis_thaliana.TAIR10.34.bed -i bam/Shape02.bam
Reading reference gene model GTF/Arabidopsis_thaliana.TAIR10.34.bed ... Done
Loading SAM/BAM file ...  Total 200000 usable reads were sampled


This is PairEnd Data
Fraction of reads failed to determine: 0.0315
Fraction of reads explained by "1++,1--,2+-,2-+": 0.4769
Fraction of reads explained by "1+-,1-+,2++,2--": 0.4916


root@featurecount_docker:/home/test# /home/software/subread-2.0.3-source/bin/featureCounts \
> -s 0 -p -t exon -g gene_id \
> -a GTF/Arabidopsis_thaliana.TAIR10.34.gtf \
> -o result/Shape02.featurecounts.exon.txt bam/Shape02.bam

        ==========     _____ _    _ ____  _____  ______          _____
        =====         / ____| |  | |  _ \|  __ \|  ____|   /\   |  __ \
          =====      | (___ | |  | | |_) | |__) | |__     /  \  | |  | |
            ====      \___ \| |  | |  _ <|  _  /|  __|   / /\ \ | |  | |
              ====    ____) | |__| | |_) | | \ \| |____ / ____ \| |__| |
        ==========   |_____/ \____/|____/|_|  \_\______/_/    \_\_____/
          v2.0.3

//========================== featureCounts setting ===========================\\
||                                                                            ||
||             Input files : 1 BAM file                                       ||
||                                                                            ||
||                           Shape02.bam                                      ||
||                                                                            ||
||             Output file : Shape02.featurecounts.exon.txt                   ||
||                 Summary : Shape02.featurecounts.exon.txt.summary           ||
||              Paired-end : yes                                              ||
||        Count read pairs : no                                               ||
||              Annotation : Arabidopsis_thaliana.TAIR10.34.gtf (GTF)         ||
||      Dir for temp files : result                                           ||
||                                                                            ||
||                 Threads : 1                                                ||
||                   Level : meta-feature level                               ||
||      Multimapping reads : not counted                                      ||
|| Multi-overlapping reads : not counted                                      ||
||   Min overlapping bases : 1                                                ||
||                                                                            ||
\\============================================================================//

//================================= Running ==================================\\
||                                                                            ||
|| Load annotation file Arabidopsis_thaliana.TAIR10.34.gtf ...                ||
||    Features : 313952                                                       ||
||    Meta-features : 32833                                                   ||
||    Chromosomes/contigs : 7                                                 ||
||                                                                            ||
|| Process BAM file Shape02.bam...                                            ||
||    Paired-end reads are included.                                          ||
||    The reads are assigned on the single-end mode.                          ||
||    Total alignments : 2730443                                              ||
||    Successfully assigned alignments : 2559170 (93.7%)                      ||
||    Running time : 0.04 minutes                                             ||
||                                                                            ||
|| Write the final count table.                                               ||
|| Write the read assignment summary.                                         ||
||                                                                            ||
|| Summary of counting results can be found in file "result/Shape02.featurec  ||
|| ounts.exon.txt.summary"                                                    ||
||                                                                            ||
\\============================================================================//

root@featurecount_docker:/home/test# cat result/Shape02.featurecounts.exon.txt | awk '$1 == "AT1G09530" {print $0}'
AT1G09530       1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1;1   3075768;3075768;3075768;3076401;3076401;3076401;3076459;3076459;3076459;3077173;3077173;3077173;3077173;3077378;3077378;3077378;3077378;3077378;3077378;3078346;3078346;3078346;3078346;3078346;3078346;3078545;3078545;3078545;3078545;3078545;3078545;3078843;3078843;3078843;3078843;3078843;3078843;3078984;3078984;3078984;3078984;3078984;3078984 3075852;3075852;3075852;3077286;3076808;3076748;3076808;3077286;3076748;3077286;3077286;3077286;3077286;3078257;3078257;3078257;3078257;3078257;3078257;3078453;3078453;3078453;3078453;3078453;3078453;3078610;3078610;3078610;3078610;3078610;3078610;3078908;3078908;3078908;3078908;3078908;3078908;3079544;3079544;3079544;3079654;3079654;3079654 +;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+;+   2762    86
```
所以共43个counts


# 4


一个bash文件merge.sh
```bash
#!/bin/bash

# 设置工作目录
WORK_DIR="tumor-transcriptome-demo"

# 合并所有文件
echo "Geneid,Sample,Count" > merged_counts.csv

# 遍历每个文件夹
for folder in COAD ESCA READ; do
    i=1
    for file in "$WORK_DIR/$folder"/*.txt; do
        # 提取文件名作为样本名
        sample="${folder}_${i}"
        i=$((i+1))
        # 提取基因ID和计数数据
        awk -v sample="$sample" 'NR>2 {print $1 "," sample "," $7}' "$file" >> merged_counts.csv
    done
done

echo "合并完成，结果保存在 merged_counts.csv"
```

```bash
bash merge.sh  #执行bash文件
```

转用R语言
```R
library(tidyverse)
library(pheatmap)
library(reshape2)
library(edgeR)

merged_data <- read.csv("E:/LiuXing/bioinfo_KunlinDu_featurecount_share/merged_counts.csv")


head(merged_data)
counts = dcast(merged_data, formula = Geneid~Sample)
head(counts)
dim(counts)
counts[is.na(counts)] <- 0
write.csv(counts, file = 'E:/LiuXing/bioinfo_KunlinDu_featurecount_share/merged_reshaped_counts_1.csv')



# CPM.matrix <- t(1000000*t(counts)/colSums(counts))
# log10.CPM.matrix <- log10(CPM.matrix+1) # 1 为pseudocount, 避免count为0时对数未定义的情况 

y <- DGEList(counts = counts) # 定义edgeR用于存储基因表达信息的DGEList对象
CPM.matrix <- edgeR::cpm(y,log=F) # 计算CPM
log10.CPM.matrix <- log10(CPM.matrix+1) # 1 为pseudocount, 避免count为0时对数未定义的情况 

z.scores <- (log10.CPM.matrix - rowMeans(log10.CPM.matrix))/apply(log10.CPM.matrix,1,sd)
# apply(log10.CPM.matrix,1,sd)表示计算每行(1表示行,2表示列)的标准差(sd函数)
# rowMeans(log10.CPM.matrix)和apply(log10.CPM.matrix,1,mean)效果是一样的

is.na(z.scores)[which()]
z.scores[is.na(z.scores)] <- 0
dim(z.scores)
class(z.scores)

pheatmap(z.scores,
         scale = "row",
         cluster_rows = T,
         cluster_cols = F,
         show_rownames = T,
         show_colnames = F
         )
```

从热图看，COAD和READ更像
<img width="1440" alt="上机2 1_heatmap_1" src="https://github.com/user-attachments/assets/d6bd365c-305c-494b-9efe-708efe177d11" />



# 2.3

## 1

在传统的假设检验中，单个检验的显著性水平或I型错误率（错误拒绝原假设的概率）为计算出的p value，但随着检验次数的增加，错误拒绝原假设的概率（即I型错误率）大大增加
多重检验校正是一种统计学方法，用于在同时进行多个假设检验时控制错误发现的可能性。

常见的多重检验方法包括：  
Bonferroni 校正：将显著性水平除以检验次数（例如，将显著性水平从 0.05 调整为 0.05/N，其中 N 是检验次数）。
Benjamini-Hochberg 方法：控制假发现率（FDR），允许一定比例的假阳性结果，但更宽松。（假设针对10000个基因进行了统计检验，对所有的原始P-value进行由小到大的排序分别为p1, p2, ..., p10000，校正后的FDR为：p1*10000/1, p2*10000/2, ..., p10000*10000/10000。与Bonferroni correction一致的地方是都乘以了检测总数，不一致的地方是BH算法在此基础上除去了各个原始p-value的排序值。）

P value（p 值）
p 值是统计学中用于衡量假设检验结果显著性的指标。它表示在原假设为真的情况下，观察到当前数据（或更极端数据）的概率。p 值越小，说明原假设越不可能成立，结果越显著。

q value（q 值，也叫 FDR）
q 值是基于假发现率（False Discovery Rate, FDR）的校正方法。q-value可以简单理解为表示p-value产生假阳性的概率，当q-value < 0.05时，p-value显著的假阳性小于0.05。

q值（q-value）是p值校正后的结果。

可定义为：多重假设检验过程中，错误拒绝（拒绝真的原假设（零假设））的个数占所有拒绝的原假设个数的比例的期望值（也是代表出错率）。


## 2
edge R在normalization中
1. 移除所有未转录/未测得的基因（即在全部样本中reads数均为0的基因）
2. 选择参考样本，随后会用这个样本来归一化其他样本，类似于qPCR中的参考基因
   如何寻找参考样本：
   2.1 用总reads数校正每个样本
   2.2 计算每个样本各自的75%百分位数
   2.3 计算平均75%百分位数
   2.4 找出最接近于平均75%百分位数的样本
3. 计算标准化因子：在这一步骤中，会选择一些基因集来创建标准化因子，计算的过程是针对”参考样本“，分别选择其余的样本的基因集来创建标准化因子，也就是说不同的样本创建的标准化因子的基因集不同：
   3.1 过滤偏倚基因：通过倍数差异的log转换，公式是log fold differences = log2(Reference/sample)，所以如果Reference中的基因值较高，为正，否则为负。然后设置一个阈值，如果得数超过这个阈值就会被剔除
   3.2 计算几何均数
   3.3 计算代表基因集
   3.4 计算加权均数：计算代表基因集的log fold的加权平均数，在edgeR中，这个log fold的加权平均数称为weighted trimmed mean of the log2 ratio，因为这些数据已经剔除掉了(trim)那些表达比较极端的基因
   3.5 将加权log2 fold值转换为真值，得到原始标准化因子
   3.6 原始标准化因子的中心化，即用每个值除以所有原始标准化因子的几何均数，得到edge R标准化因子
4. 将表达矩阵中的数值除以这个标准化因子


DESeq2
1. 对reads数取自然对数
2. 求所有样本中，相同基因对数的均值
3. 去除掉Infinity，(会把独特转录的基因都剔除掉)
4. 矩阵减均值，（用的是对数转换后的数值相减，所以本质上是基因表达平均值为参考做了归一化）
5. 计算每个样本的中位数
6. 将中位数转换为真数，计算每个样本最终的标准化因子
7. 原始reads数除以标准化因子


## 3/4/5

```R
library(DESeq2)
library(edgeR)


filepath <- file.choose()
filepath <- "E:\\LiuXing\\bioinfo_tsinghua\\DATA_FOR_R\\count_exon.txt"
raw.counts <- read.table(filepath, sep = '\t', header = T, row.names = 1)
head(raw.counts)
MUT.raw.counts <- raw.counts[, c("UD1_1", "UD1_2", "UD1_3",
                             "UD0_1", "UD0_2", "UD0_3")]
MUT.filter.counts <- MUT.raw.counts[rowMeans(MUT.raw.counts)>5,]




#################DESeq2
conditions <- factor(c(rep("Con", 3), rep("Trea", 3)),
                     levels = c("Con", "Trea"))
colData <- data.frame(row.names = colnames(MUT.filter.counts),
                      conditions = conditions)

dds <- DESeqDataSetFromMatrix(MUT.filter.counts,
                              colData = colData,
                              design = ~conditions)
dds2 <- DESeq(dds)
res <- results(dds2)
head(res)


write.table(res, 
            "E:\\LiuXing\\bioinfo_tsinghua\\DATA_FOR_R\\DESeq_MUT_LIGHT_VS_DARK.txt",
            sep = '\t',
            row.names = T,
            quote = F)

DESeq.diff.table <- subset(res, padj<0.05 & abs(log2FoldChange)>1)
head(DESeq.diff.table)
write.table(DESeq.diff.table, 
            "E:\\LiuXing\\bioinfo_tsinghua\\DATA_FOR_R\\DESeq_DIFF_GENE_MUT_LIGHT_VS_DARK.txt",
            sep = '\t',
            row.names = T,
            quote = F)


####################edgeR

# 创建 DGEList 对象，用于存储基因表达数据和组信息
y <- DGEList(counts = MUT.filter.counts,
             group = conditions)

#自动过滤，去除低表达基因
keep <- filterByExpr(y)
table(keep)

#从DGEList对象中筛选出符合条件的基因
y <- y[keep, , keep.lib.size = F]
# 归一化，TMM 方法
y <- calcNormFactors(y, method = "TMM")

dge = y
# 创建design矩阵，用于指定差异分析模型
design <- model.matrix(~0+factor(conditions))
rownames(design) <- colnames(dge)
colnames(design) <- levels(conditions)

# 估计数据的离散度 —— common离散度、trended离散度、tagwise离散度
dge <- estimateGLMCommonDisp(dge, design)
dge <- estimateGLMTrendedDisp(dge, design)
dge <- estimateGLMTagwiseDisp(dge, design)
#等同于
dge <- estimateDisp(dge, design = design)


fit <- glmFit(dge, design = design)
lrt <- glmLRT(fit, contrast = c(-1, 1))

edgeR.diff.table <- topTags(lrt, n=nrow(dge))$table
edgeR.diff.table.sign <- subset(edgeR.diff.table, abs(logFC)>1 & FDR<0.05)

write.table(edgeR.diff.table.sign, 
            "E:\\LiuXing\\bioinfo_tsinghua\\DATA_FOR_R\\edgeR_DIFF_GENE_MUT_LIGHT_VS_DARK.txt",
            sep = '\t',
            row.names = T,
            quote = F)



diff.gene.list <- list(DESeq2 = rownames(DESeq.diff.table) %>% as.vector(), 
                    edgeR = rownames(edgeR.diff.table.sign) %>% as.vector())


library(ggvenn)


p.venn <- 
  ggvenn(
    data = diff.gene.list,      # 数据列表
    columns = NULL,           # 对选中的列名绘图，最多选择4个，NULL为默认全选
    show_elements = F,        # 当为TRUE时，显示具体的交集情况，而不是交集个数
    label_sep = "\n",         # 当show_elements = T时生效，分隔符 \n 表示的是回车的意思
    show_percentage = T,      # 显示每一组的百分比
    digits = 1,               # 百分比的小数点位数
    fill_color = c("#A51c36","#7abbdb"), #, "#FF8C00", "#80FF00"), # 填充颜色
    fill_alpha = 0.5,         # 填充透明度
    stroke_color = "white",   # 边缘颜色
    stroke_alpha = 0.5,       # 边缘透明度
    stroke_size = 0.5,        # 边缘粗细
    stroke_linetype = "solid", # 边缘线条 # 实线：solid  虚线：twodash longdash 点：dotdash dotted dashed  无：blank
    set_name_color = "black", # 组名颜色
    set_name_size = 4,        # 组名大小
    text_color = "black",     # 交集个数颜色
    text_size = 4             # 交集个数文字大小
  )






edgeR.diff.table.subset <- edgeR.diff.table %>% 
  subset(FDR < 0.05)

edgeR.diff.table.subset[order(edgeR.diff.table.subset$logFC),]

edgeR.diff.table.forheatmap <- rbind(head(edgeR.diff.table.subset, 10),
                                 tail(edgeR.diff.table.subset,10)) %>% 
  as.data.frame()

cpm <- cpm(dge)
logCPM <- log10(cpm+1)

heatmap.data <- logCPM[rownames(edgeR.diff.table.forheatmap),] %>% as.matrix()
#heatmap.data <- apply(heatmap.data, 2, as.numeric)



library(pheatmap)
# 绘制热图
p2 <- pheatmap(heatmap.data,  
               #color = colorRampPalette(c('blue','white','red'))(100), 
               border_color = "black",  
               scale = "row", 
               cluster_rows = T, 
               cluster_cols = FALSE, 
               legend = TRUE, 
               legend_breaks = c(-1, 0, 1), 
               legend_labels = c("low","","heigh"), 
               show_rownames = TRUE, 
               show_colnames = TRUE, 
               fontsize = 8, 
               #annotation_row = anno_row,   #添加行注释信息
               #annotation_col = anno_col,   #添加列注释信息
               annotation_legend = TRUE,    #是否显示注释信息图例
               annotation_names_row = TRUE, #是否显示行注释的名称
               annotation_names_col = TRUE  #是否显示列注释的名称
)
```

![image](https://github.com/user-attachments/assets/a1adc8c3-3868-44be-a740-08737d3dfc1b)

![image](https://github.com/user-attachments/assets/98a8751f-6274-4f8f-9cbb-c92576945514)

