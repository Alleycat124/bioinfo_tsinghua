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
<img width="1440" alt="上机2 1_heatmap_1" src="https://github.com/user-attachments/assets/d6bd365c-305c-494b-9efe-708efe177d11" />

