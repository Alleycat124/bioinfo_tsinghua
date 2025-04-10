# 1

**是单端测序**，代码结果如下

```bash
root@bioinfo_docker:/home/test/samtools_bedtools# ls -al
total 11944
drwxr-xr-x 3 test test     4096 Apr 10 12:52 .
drwxr-xr-x 1 test test     4096 Apr 10 12:17 ..
drwxr-xr-x 2 test test     4096 Apr 10 12:19 bin
-rwxr-xr-x 1 root root 12211134 Apr 10 12:52 COAD.ACTB.bam
root@bioinfo_docker:/home/test/samtools_bedtools# samtools flagstat COAD.ACTB.bam
185650 + 0 in total (QC-passed reads + QC-failed reads)
180727 + 0 primary
4923 + 0 secondary
0 + 0 supplementary
0 + 0 duplicates
0 + 0 primary duplicates
185650 + 0 mapped (100.00% : N/A)
180727 + 0 primary mapped (100.00% : N/A)
0 + 0 paired in sequencing
0 + 0 read1
0 + 0 read2
0 + 0 properly paired (N/A : N/A)
0 + 0 with itself and mate mapped
0 + 0 singletons (N/A : N/A)
0 + 0 with mate mapped to a different chr
0 + 0 with mate mapped to a different chr (mapQ>=5)
```

# 2
Secondary Alignment（次要比对）指的是一个读取（read）在比对过程中，可能匹配到多个位置的情况。这种情况通常发生在以下几种情况：

**重复区域**：如果某个基因组区域有许多相似的序列，比如重复序列或转座子，那么同一条读取可能会比对到这些多个位置。这时，软件会给出一个主要比对和多个次要比对。

**多重比对**：对于某些读取，由于结构变异或参考基因组的特性，它们可能符合多个位置的比对条件，因此被标记为次要比对，这通常也表示这些读取在该位点不是唯一的。


**提供的文件中有4923个secondary alignment**
代码如下，与第一题中代码匹配

```bash
root@bioinfo_docker:/home/test/samtools_bedtools# samtools view -f 256 COAD.ACTB.bam |
wc -l
4923
```


# 3

```bash
root@bioinfo_docker:/home/test/samtools_bedtools# cat hg38.ACTB.gff | awk '$3 == "gene" {print $0}' > ACTB_gene.gff
root@bioinfo_docker:/home/test/samtools_bedtools# cat hg38.ACTB.gff | awk '$3 == "exon" {print $0}' > ACTB_exon.gff
root@bioinfo_docker:/home/test/samtools_bedtools# ls
ACTB_exon.gff  ACTB_gene.gff  bin  COAD.ACTB.bam  hg38.ACTB.gff
root@bioinfo_docker:/home/test/samtools_bedtools# bedtools subtract -a ACTB_gene.gff -b ACTB_exon.gff
chr7    HAVANA  gene    5528186 5528280 .       -       .       ID=ENSG00000075624.17;gene_id=ENSG00000075624.17;gene_type=protein_coding;gene_name=ACTB;level=2;hgnc_id=HGNC:132;havana_gene=OTTHUMG00000023268.12
chr7    HAVANA  gene    5529983 5530523 .       -       .       ID=ENSG00000075624.17;gene_id=ENSG00000075624.17;gene_type=protein_coding;gene_name=ACTB;level=2;hgnc_id=HGNC:132;havana_gene=OTTHUMG00000023268.12
chr7    HAVANA  gene    5530628 5540675 .       -       .       ID=ENSG00000075624.17;gene_id=ENSG00000075624.17;gene_type=protein_coding;gene_name=ACTB;level=2;hgnc_id=HGNC:132;havana_gene=OTTHUMG00000023268.12
chr7    HAVANA  gene    5540772 5561851 .       -       .       ID=ENSG00000075624.17;gene_id=ENSG00000075624.17;gene_type=protein_coding;gene_name=ACTB;level=2;hgnc_id=HGNC:132;havana_gene=OTTHUMG00000023268.12
chr7    HAVANA  gene    5561950 5562389 .       -       .       ID=ENSG00000075624.17;gene_id=ENSG00000075624.17;gene_type=protein_coding;gene_name=ACTB;level=2;hgnc_id=HGNC:132;havana_gene=OTTHUMG00000023268.12
chr7    HAVANA  gene    5562829 5563713 .       -       .       ID=ENSG00000075624.17;gene_id=ENSG00000075624.17;gene_type=protein_coding;gene_name=ACTB;level=2;hgnc_id=HGNC:132;havana_gene=OTTHUMG00000023268.12
root@bioinfo_docker:/home/test/samtools_bedtools#

root@bioinfo_docker:/home/test/samtools_bedtools# bedtools subtract -a ACTB_gene.gff -b ACTB_exon.gff > ACTB_introns.bed
root@bioinfo_docker:/home/test/samtools_bedtools# cat ACTB_introns.bed
chr7    HAVANA  gene    5528186 5528280 .       -       .       ID=ENSG00000075624.17;gene_id=ENSG00000075624.17;gene_type=protein_coding;gene_name=ACTB;level=2;hgnc_id=HGNC:132;havana_gene=OTTHUMG00000023268.12
chr7    HAVANA  gene    5529983 5530523 .       -       .       ID=ENSG00000075624.17;gene_id=ENSG00000075624.17;gene_type=protein_coding;gene_name=ACTB;level=2;hgnc_id=HGNC:132;havana_gene=OTTHUMG00000023268.12
chr7    HAVANA  gene    5530628 5540675 .       -       .       ID=ENSG00000075624.17;gene_id=ENSG00000075624.17;gene_type=protein_coding;gene_name=ACTB;level=2;hgnc_id=HGNC:132;havana_gene=OTTHUMG00000023268.12
chr7    HAVANA  gene    5540772 5561851 .       -       .       ID=ENSG00000075624.17;gene_id=ENSG00000075624.17;gene_type=protein_coding;gene_name=ACTB;level=2;hgnc_id=HGNC:132;havana_gene=OTTHUMG00000023268.12
chr7    HAVANA  gene    5561950 5562389 .       -       .       ID=ENSG00000075624.17;gene_id=ENSG00000075624.17;gene_type=protein_coding;gene_name=ACTB;level=2;hgnc_id=HGNC:132;havana_gene=OTTHUMG00000023268.12
chr7    HAVANA  gene    5562829 5563713 .       -       .       ID=ENSG00000075624.17;gene_id=ENSG00000075624.17;gene_type=protein_coding;gene_name=ACTB;level=2;hgnc_id=HGNC:132;havana_gene=OTTHUMG00000023268.12

root@bioinfo_docker:/home/test/samtools_bedtools# bedtools intersect -a COAD.ACTB.bam -b ACTB_introns.bed -wa -u > COAD_ACTB_introns.bam
root@bioinfo_docker:/home/test/samtools_bedtools# samtools view COAD_ACTB_introns.bam | head
UNC3-RDR300156_8:2:1:1584:8723  16      chr7    5529628 255     36M860N40M      *       0       0       GAAGAGGAGGGCGGCGATATCATCATCCATGGTGAGCTGGCGGCGGGTGTGGACGGGCGGCGGATCGGCAAAGGCG    ###########?CA@A0A?@=B@=BBCB5AEEBEBAD?FFBFFDEFF?F=FFDBFFGAGBGFGGGFFGGGEFFGGG    NH:i:1  HI:i:1  NM:i:3  MD:Z:2C2C3C66   AS:i:70 XS:A:-  RG:Z::100608_UNC3-RDR300156_0008.2
UNC3-RDR300156_8:2:1:1605:10563 0       chr7    5528179 255     7M95N69M        *       0       0       TCCATGCCCAGGAAGGAAGGCTGGAAGAGTGCCTCAGGGCAGCGGAACCGCTCATTGCCAATGGTGATGACCTGGC    BBEBEEEEEEEEED?EE?EDEEED:EDEEEDEEEE?ACDD=BDBCBEEE=D5C=?BBDD=B5@B??B,@C??<;>D    NH:i:1  HI:i:1  NM:i:0  MD:Z:76 AS:i:76 XS:A:-  RG:Z::100608_UNC3-RDR300156_0008.2
UNC3-RDR300156_8:2:1:1831:17967 16      chr7    5528145 255     41M95N35M       *       0       0       GGAGTTGAAGGTAGTTTCGTGGATGCCACAGGACTCCATGCCCAGGAAGGAAGGCTGGAAGAGTGCCTCAGGGCAG    E:BEBDDBAEEFAFADEEFEGEDBGGEEEFAGDGF?GGFFGGEEGGEGGGBGGGGGGGGFGGGFFGGEGGGGGGGG    NH:i:1  HI:i:1  NM:i:0  MD:Z:76 AS:i:76 XS:A:-  RG:Z::100608_UNC3-RDR300156_0008.2
UNC3-RDR300156_8:2:1:2008:14197 0       chr7    5528115 255     71M95N5M        *       0       0       TTTGCGGATGTCCACGTCACACTTCATGATGGAGTTGAAGGTAGTTTCGTGGATGCCACAGGACTCCATGCCCAGG    GGGFGDFGGBGGGAGFFFFFAFFFFFFDFFC=@CBFDEE?BEDEFEFFEFADBAAEE:B=B@@B5EE@@@CDB1@>    NH:i:1  HI:i:1  NM:i:0  MD:Z:76 AS:i:76 XS:A:-  RG:Z::100608_UNC3-RDR300156_0008.2
UNC3-RDR300156_8:2:1:2125:4113  16      chr7    5529605 255     59M860N17M      *       0       0       TTGCACATGCCGGGGCCGTTGTCGACGACGAGCGCGGCGATATCATCATCCATGGTGAGCTGGCGGCGGGTGTGGA    ####################@??A:?B?AA=EDEBEEEEEEDEDEEEE:EEDCDD5DCD?6FGEGGGGFGGGDGGF    NH:i:1  HI:i:1  NM:i:1  MD:Z:13A62      AS:i:74 XS:A:-  RG:Z::100608_UNC3-RDR300156_0008.2
UNC3-RDR300156_8:2:1:2149:11459 16      chr7    5528177 255     9M95N67M        *       0       0       ACTCCATGCCCAGGAAGGAAGGCTGGAAGAGTGCCTCAGGGCAGCGGAACCGCTCATTGCCAATGGTGATGACCTG    A@CC5ABE:CCAGEEEGEEBGFEEGEEAFFGBEEEAGEGGGEFGGGGGGGGGGGFGGDGEFGGFGGGGGEGGGGGG    NH:i:1  HI:i:1  NM:i:0  MD:Z:76 AS:i:76 XS:A:-  RG:Z::100608_UNC3-RDR300156_0008.2
UNC3-RDR300156_8:2:1:2303:16212 0       chr7    5528169 255     6S17M95N53M     *       0       0       CGACGGGCCACAGGACTCCATGCCCAGGAAGGAAGGCTGGAAGAGTGCCTCAGGGCAGCGGAACCGCTCATTGCCA    GGGGGAGDEFGGGFGEEGGFG=GFGEGGEDFF@?DFADFF?DFEACDDECC?DCDDAE=?C:CB??=6?589??>3    NH:i:1  HI:i:1  NM:i:0  MD:Z:70 AS:i:70 XS:A:-  RG:Z::100608_UNC3-RDR300156_0008.2
UNC3-RDR300156_8:2:1:2425:10866 0       chr7    5528162 255     24M95N52M       *       0       0       CGTGGATGCCACAGGACTCCATGCCCAGGAAGGAAGGCTGGAAGAGTGCCTCAGGGCAGCGGAACCGCTCATTGCC    EEEEEFEEFFFFAFEEGDGGFDBFFEEEEEBEEEBAAEAB:5????<?BC?A@??<B?4==<:?@B:@<@4::8:=    NH:i:1  HI:i:1  NM:i:0  MD:Z:76 AS:i:76 XS:A:-  RG:Z::100608_UNC3-RDR300156_0008.2
UNC3-RDR300156_8:2:1:2817:15795 16      chr7    5528185 255     1M95N75M        *       0       0       CCCAGGAAGGAAGGCTGGAAGAGTGCCTCAGGGCAGCGGAACCGCTCATTGCCAATGGTGATGACCTGGCCGTCAG    B:@:AA25@@2@>5BBAD@AD?EBEBB=BAEBEB5EBBBB@?AB=DBC?=EBFDDBFFDFFEFCFFGGADDGFFFG    NH:i:1  HI:i:1  NM:i:0  MD:Z:76 AS:i:76 XS:A:-  RG:Z::100608_UNC3-RDR300156_0008.2
UNC3-RDR300156_8:2:1:3080:1143  0       chr7    5528149 255     37M95N39M       *       0       0       TTGAAGGTAGTTTCGTGGATGCCACAGGACTCCATGCCCAGGAAGGAAGGCTGGAAGAGTGCCTCAGGGCAGCGGA    FFFFFDGGEGGGGGGGGFAGEFFDFGEFEDGGGEGEFFFDFEA?EGAEEBEEE:AB?@@@BDB@D?@BB@5:?:?#    NH:i:1  HI:i:1  NM:i:0  MD:Z:76 AS:i:76 XS:A:-  RG:Z::100608_UNC3-RDR300156_0008.2

root@bioinfo_docker:/home/test/samtools_bedtools# samtools fastq COAD_ACTB_introns.bam > COAD_ACTB_introns.fastq
[M::bam2fq_mainloop] discarded 0 singletons
[M::bam2fq_mainloop] processed 15132 reads
root@bioinfo_docker:/home/test/samtools_bedtools# cat COAD_ACTB_introns.fastq | head
@UNC3-RDR300156_8:2:1:1584:8723
CGCCTTTGCCGATCCGCCGCCCGTCCACACCCGCCGCCAGCTCACCATGGATGATGATATCGCCGCCCTCCTCTTC
+
GGGFFEGGGFFGGGFGBGAGFFBDFF=F?FFEDFFBFF?DABEBEEA5BCBB=@B=@?A0A@AC?###########
@UNC3-RDR300156_8:2:1:1605:10563
TCCATGCCCAGGAAGGAAGGCTGGAAGAGTGCCTCAGGGCAGCGGAACCGCTCATTGCCAATGGTGATGACCTGGC
+
BBEBEEEEEEEEED?EE?EDEEED:EDEEEDEEEE?ACDD=BDBCBEEE=D5C=?BBDD=B5@B??B,@C??<;>D
@UNC3-RDR300156_8:2:1:1831:17967
CTGCCCTGAGGCACTCTTCCAGCCTTCCTTCCTGGGCATGGAGTCCTGTGGCATCCACGAAACTACCTTCAACTCC
```


# 4
```bash
root@bioinfo_docker:/home/test/samtools_bedtools# bedtools genomecov -ibam COAD.ACTB.bam -bga -split -g ACTB_genes.bed > ACTB_coverage.bedgraph

*****
*****WARNING: Genome (-g) files are ignored when BAM input is provided.
*****
root@bioinfo_docker:/home/test/samtools_bedtools# cat ACTB_coverage.bedgraph | head
chr7    0       5045717 0
chr7    5045717 5045731 1
chr7    5045731 5058689 0
chr7    5058689 5058695 1
chr7    5058695 5072542 0
chr7    5072542 5072543 2
chr7    5072543 5072554 5
chr7    5072554 5073147 0
chr7    5073147 5073157 1
chr7    5073157 5077437 0
```

人类基因组大小为3.1Gb，基本组成为24条染色体+线粒体  
更进一步的
![](https://i-blog.csdnimg.cn/blog_migrate/30732d349bde677a8dbf849665f41232.png)
