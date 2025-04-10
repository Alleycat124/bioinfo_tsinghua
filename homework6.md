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



人类基因组大小为3.1Gb，基本组成为24条染色体+线粒体  
更进一步的
![](https://i-blog.csdnimg.cn/blog_migrate/30732d349bde677a8dbf849665f41232.png)
