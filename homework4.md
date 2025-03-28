# 1
bowtie中利用了BWT的以下性质提高了运算速度：  
**可逆性**：BWT是一种可逆的字符串排列方法，能够根据转换后的序列还原出原始序列，这使得在比对过程中无需存储整个参考基因组序列，只需存储BWT转换后的序列即可。  
**压缩性**：BWT转换后的序列通常包含许多重复的字符，这使得后续的压缩更加有效，从而减少了存储空间的需求。  

bowtie通过以下策略优化了对内存的需求：  
**构建FM索引**：bowtie采用Burrows-Wheeler算法对参考基因组建立索引，即FM索引。FM索引是一种基于BWT的索引结构，它能够在较低的内存占用下实现高效的字符串搜索。bowtie建立的人类基因组索引在硬盘上的大小为2.2GB，在比对时的内存为1.3GB。  
**双索引策略**：bowtie引入了双索引策略，即对正向参考基因组和反向参考基因组分别进行BWT转换，形成“Forward index”和“Mirror index”。在比对过程中，根据reads错配的位置，分别加载不同的索引到内存中，避免过度回溯，从而减少内存的使用。  



```bash
bowtie -v 2 -m 10 --best --strata BowtieIndex/YeastGenome -f THA2.fa -S THA2.sam

test@bioinfo_docker:~/mapping$ cat THA2.sam | grep -v "@" | awk '{print $3}' | sort | uniq -c
     92 *
     18 chrI
     51 chrII
     15 chrIII
    194 chrIV
     25 chrIX
     12 chrmt
     33 chrV
     17 chrVI
    125 chrVII
     68 chrVIII
     71 chrX
     56 chrXI
    169 chrXII
     67 chrXIII
     58 chrXIV
    101 chrXV
     78 chrXVI

```
