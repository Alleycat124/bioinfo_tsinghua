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






人类基因组大小为3.1Gb，基本组成为24条染色体+线粒体  
更进一步的
![](https://i-blog.csdnimg.cn/blog_migrate/30732d349bde677a8dbf849665f41232.png)
