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


# KEGG
![image](https://github.com/user-attachments/assets/10664ee5-b1de-492c-93ab-1938b14737a1)
