```R
filepath2 <- "E:\\LiuXing\\bioinfo_tsinghua\\DATA_FOR_R\\wt.light.vs.dark.all.txt"
diff.gene <- read.table(filepath2, sep = '\t', header = T, row.names = 1)
diff.gene.filter <- subset(diff.gene, 
                           abs(log2FoldChange>1) & padj<0.05)

write.table(rownames(diff.gene.filter),
            "E:\\LiuXing\\bioinfo_tsinghua\\DATA_FOR_R\\DIFF_GENENAME_MUT_LIGHT_VS_DARK.txt",
            sep = '\t',
            row.names = F,
            quote = F)
```

# GO
![image](https://github.com/user-attachments/assets/bca92785-1bec-4023-b8eb-73fe50c4f679)

![image](https://github.com/user-attachments/assets/0a8adb48-2ca3-4b7b-bee3-5de53f4467d9)


**Fold Enrichment**  
Fold Enrichment = \frac{goal ratio}{background ration}

目标基因集：通常是感兴趣的基因集合，例如差异表达基因。
背景基因集：通常是整个基因组或实验中所有可用基因的集合。
计算步骤：
统计目标基因集中对应GO术语的基因ratio。
统计背景基因集中的该GO术语的ratio。
使用该GO术语目标基因集的比例与背景基因集中的比例，来计算富集倍数。若Fold Enrichment>1，说明目标基因集在该GO术语中高度富集。

**P value**  
![image](https://github.com/user-attachments/assets/4ec569d9-7221-4cf9-b7ac-1c124f47ce08)

P-value通常是通过超几何分布或二项分布计算的，具体依赖于选择的方法。  
P value是当原假设为真时所得到的样本观察结果或更极端结果出现的概率。如果P值很小（如小于0.05），说明这种情况的发生的概率很小，而如果出现了，根据小概率原理，我们就有理由拒绝原假设。也就是我们可以认为鉴定的基因和通路有比较强的联系。

**FDR**
GO分析中，虽然P value是一个重要的指标，但是会存在多重检验问题：  
当同时进行多次检验时（例如分析许多GO术语），即使P-value很小，也可能是偶然偏差造成的。因此，单独依赖P-value会增加假阳性的风险。

# KEGG
![image](https://github.com/user-attachments/assets/10664ee5-b1de-492c-93ab-1938b14737a1)


GO分析中的结果主要是与某种生物功能有关的通路，比如：
功能注释：强调基因的功能及其参与的生物过程，如某种疾病  
高层次的生物学分类：通过将基因与已知的功能分类进行比较，提供关于基因在生物体内如何操作的全面视角。

而KEGG中的结果则主要是生物中的代谢通路，重点关注在特定生物代谢和信号传导通路中的作用
