# 1
bowtie中利用了BWT的以下性质提高了运算速度：  
**可逆性**：BWT是一种可逆的字符串排列方法，能够根据转换后的序列还原出原始序列，这使得在比对过程中无需存储整个参考基因组序列，只需存储BWT转换后的序列即可。  
**压缩性**：BWT转换后的序列通常包含许多重复的字符，这使得后续的压缩更加有效，从而减少了存储空间的需求。  

bowtie通过以下策略优化了对内存的需求：  
**构建FM索引**：bowtie采用Burrows-Wheeler算法对参考基因组建立索引，即FM索引。FM索引是一种基于BWT的索引结构，它能够在较低的内存占用下实现高效的字符串搜索。bowtie建立的人类基因组索引在硬盘上的大小为2.2GB，在比对时的内存为1.3GB。  
**双索引策略**：bowtie引入了双索引策略，即对正向参考基因组和反向参考基因组分别进行BWT转换，形成“Forward index”和“Mirror index”。在比对过程中，根据reads错配的位置，分别加载不同的索引到内存中，避免过度回溯，从而减少内存的使用。  


# 2
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


# 3
## 3.1
CIGAR（Compact Idiosyncratic Gapped Alignment Report）字符串是SAM/BAM文件中用于描述比对的序列如何与参考序列对齐的字段。它由一系列操作符和相应的长度组成，每个操作符表示比对中的不同操作。常见的操作符包括：  
M：匹配或不匹配（即序列与参考序列对齐的位置）。  
I：插入（即序列中相对于参考序列的插入）。  
D：删除（即参考序列中相对于序列的删除）。  
N：跳过（即参考序列中未对齐的区域）。  
S：软剪切（即序列中未对齐的部分，但在输出中保留）。  
H：硬剪切（即序列中未对齐的部分，且在输出中不保留）。  
P：填充（即在比对中插入的填充）。  
=：匹配（与参考序列匹配的碱基）。  
X：不匹配（与参考序列不匹配的碱基）。  
![CIGAR字符含义](https://pic4.zhimg.com/v2-0b25ee191db5ba816b403b8fb2914f79_1440w.jpg"CIGAR字符含义")  

例如，CIGAR字符串9M1D2M表示序列中有9个匹配或不匹配的碱基，然后是1个删除，接着是2个匹配或不匹配的碱基。

## 3.2
"Soft clip"是指序列中的一部分在比对过程中未与参考序列对齐，但在输出的SAM/BAM文件中仍然保留这部分序列。这通常发生在序列的两端，可能是因为这部分序列无法与参考序列可靠地对齐。  
在CIGAR字符串中，"soft clip"用操作符S表示。例如，CIGAR字符串3S2M4S表示序列中有3个碱基被软剪切，然后是2个匹配或不匹配的碱基，最后是4个碱基被软剪切。  

## 3.3
reads的mapping quality（映射质量）是SAM/BAM文件中的一个字段，用于表示比对算法对reads映射位置的置信度。它是一个0到255之间的整数，值越高表示比对算法对映射位置的置信度越高。  
映射质量反映了比对的可靠性。例如，一个映射质量为60的reads比一个映射质量为10的reads更可能被正确地映射到参考基因组上。低映射质量的reads可能有多个可能的映射位置，或者比对算法对其映射位置不太确定。

## 3.4
可以


# 4
```bash
root@bioinfo_docker:/home/test/mapping# wget http://hgdownload.soe.ucsc.edu/goldenPath/sacCer3/bigZips/sacCer3.fa.gz
--2025-03-31 02:42:46--  http://hgdownload.soe.ucsc.edu/goldenPath/sacCer3/bigZips/sacCer3.fa.gz
Resolving hgdownload.soe.ucsc.edu (hgdownload.soe.ucsc.edu)... 128.114.119.163
Connecting to hgdownload.soe.ucsc.edu (hgdownload.soe.ucsc.edu)|128.114.119.163|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3820548 (3.6M) [application/x-gzip]
Saving to: ‘sacCer3.fa.gz’

sacCer3.fa.gz                 100%[=================================================>]   3.64M  --.-KB/s    in 0.1s

2025-03-31 02:42:47 (36.5 MB/s) - ‘sacCer3.fa.gz’ saved [3820548/3820548]

root@bioinfo_docker:/home/test/mapping# gunzip sacCer3.fa.gz
gzip: sacCer3.fa already exists; do you wish to overwrite (y or n)? y

root@bioinfo_docker:/home/test/mapping# ./bwa/bwa index sacCer3.fa
[bwa_index] Pack FASTA... 0.13 sec
[bwa_index] Construct BWT for the packed sequence...
[bwa_index] 5.64 seconds elapse.
[bwa_index] Update BWT... 0.14 sec
[bwa_index] Pack forward-only FASTA... 0.07 sec
[bwa_index] Construct SA from BWT and Occ... 1.41 sec
[main] Version: 0.7.19-r1273
[main] CMD: ./bwa/bwa index sacCer3.fa
[main] Real time: 7.445 sec; CPU: 7.387 sec
root@bioinfo_docker:/home/test/mapping# ls -la
total 43488
drwxr-xr-x 1 test test     4096 Mar 31 02:44 .
drwxr-xr-x 1 test test     4096 Mar 25 11:11 ..
drwxr-xr-x 2 test test     4096 Sep 24  2013 BowtieIndex
drwxr-xr-x 7 test test     4096 Sep 25  2013 bowtie-src
drwxr-xr-x 5 root root     4096 Mar 28 09:20 bwa
drwxr-xr-x 3 test test     4096 Oct 23  2017 bwa-0.7.17
-rw-r--r-- 1 test test   190908 Nov  7  2017 bwa-0.7.17.tar.bz2
-rw-r--r-- 1 test test   456335 Mar 25 10:21 bwa.git
-rw-r--r-- 1 test test      233 Mar 25 10:30 dockerfile
-rwxr-xr-x 1 test test    81890 Sep 24  2013 e_coli_1000_1.fq
-rw-r--r-- 1 test test    29191 Nov  3  2019 e_coli_500.bed
-rwxrw-r-- 1 test test    41000 Nov  2  2019 e_coli_500.fq
-rw-r--r-- 1 test test    77783 Nov  2  2019 e_coli_500.sam
-rw-r--r-- 1 root root 12400379 Jan 23  2020 sacCer3.fa
-rw-r--r-- 1 root root       14 Mar 31 02:44 sacCer3.fa.amb
-rw-r--r-- 1 root root      563 Mar 31 02:44 sacCer3.fa.ann
-rw-r--r-- 1 root root 12157188 Mar 31 02:44 sacCer3.fa.bwt
-rw-r--r-- 1 root root  3039278 Mar 31 02:44 sacCer3.fa.pac
-rw-r--r-- 1 root root  6078608 Mar 31 02:44 sacCer3.fa.sa
-rwxr-xr-x 1 test test     1207 Sep 25  2013 sam2bed.pl
-rw-r--r-- 1 test test  9150483 Jan 25  2024 samtools-1.19.2.tar.bz2
-rw-r--r-- 1 test test    81783 Nov  3  2019 THA1.bed
-rwxr-xr-x 1 test test    90650 Sep 24  2013 THA1.fa
-rw-r--r-- 1 test test   284517 Nov  3  2019 THA1.sam
-rw-r--r-- 1 test test    41033 Nov  3  2019 THA2.bed
-rwxrw-r-- 1 test test    45627 Nov  2  2019 THA2.fa
-rw-r--r-- 1 test test   142663 Mar 25 05:42 THA2.sam
-rw-r--r-- 1 test test    32324 Nov  3  2019 THA2_V.bed
-rw-r--r-- 1 test test     6086 Nov  3  2019 THA2_XII.bed
root@bioinfo_docker:/home/test/mapping# ./bwa/bwa mem sacCer3.fa THA2.fa > THA2-bwa.sam
[M::bwa_idx_load_from_disk] read 0 ALT contigs
[M::process] read 1250 sequences (31877 bp)...
[M::mem_process_seqs] Processed 1250 reads in 0.012 CPU sec, 0.017 real sec
[main] Version: 0.7.19-r1273
[main] CMD: ./bwa/bwa mem sacCer3.fa THA2.fa
[main] Real time: 0.074 sec; CPU: 0.065 sec
```


# Genome Browser

```bash
root@bioinfo_docker:/home/test/mapping# ./bwa/bwa index sacCer3.fa
[bwa_index] Pack FASTA... 0.21 sec
[bwa_index] Construct BWT for the packed sequence...
[bwa_index] 5.57 seconds elapse.
[bwa_index] Update BWT... 0.18 sec
[bwa_index] Pack forward-only FASTA... 0.07 sec
[bwa_index] Construct SA from BWT and Occ... 1.50 sec
[main] Version: 0.7.19-r1273
[main] CMD: ./bwa/bwa index sacCer3.fa
[main] Real time: 7.601 sec; CPU: 7.539 sec
root@bioinfo_docker:/home/test/mapping# ./bwa/bwa mem sacCer3.fa THA2.fa > THA2-bwa.sam
[M::bwa_idx_load_from_disk] read 0 ALT contigs
[M::process] read 1250 sequences (31877 bp)...
[M::mem_process_seqs] Processed 1250 reads in 0.008 CPU sec, 0.015 real sec
[main] Version: 0.7.19-r1273
[main] CMD: ./bwa/bwa mem sacCer3.fa THA2.fa
[main] Real time: 0.036 sec; CPU: 0.028 sec
root@bioinfo_docker:/home/test/mapping# ./bwa/bwa mem sacCer3.fa THA2.fa > THA2-bwa.sam
[M::bwa_idx_load_from_disk] read 0 ALT contigs
[M::process] read 1250 sequences (31877 bp)...
[M::mem_process_seqs] Processed 1250 reads in 0.009 CPU sec, 0.015 real sec
[main] Version: 0.7.19-r1273
[main] CMD: ./bwa/bwa mem sacCer3.fa THA2.fa
[main] Real time: 0.103 sec; CPU: 0.094 sec
root@bioinfo_docker:/home/test/mapping# samtools-1.19.2/samtools view -bS THA2-bwa.sam > THA2-bwa.bam
root@bioinfo_docker:/home/test/mapping# head THA2-bwa.sam
@HD     VN:1.5  SO:unsorted     GO:query
@SQ     SN:chrI LN:230218
@SQ     SN:chrII        LN:813184
@SQ     SN:chrIII       LN:316620
@SQ     SN:chrIV        LN:1531933
@SQ     SN:chrIX        LN:439888
@SQ     SN:chrV LN:576874
@SQ     SN:chrVI        LN:270161
@SQ     SN:chrVII       LN:1090940
@SQ     SN:chrVIII      LN:562643
root@bioinfo_docker:/home/test/mapping# head THA2-bwa.bam
�BC�U�KN�@�
           B`<�x(
                 (��1���������@�Z^�C�1��!�HYD�rp�9@X%�$۴{�=��ծ����ڵ��Y8Yh�;����:g��'���ܥٗf{p�
                                                                                             ���y9G�#QU�,A��d�����*d$  ��HL)����\PR{l�fs",k�ӇI*|V��1�8�\6��'�����wͥ��{�C��!̉�wz��(�;JGn��������p��R�dt;_�j�ݷX��.��lM�m�/҉�␦
                                                                                                   ���<m57��>�^����<
                                                                                                                    "�v�␦Vl��?K������2����l)
(����T�<�#��2����'/�����?�<�/�����<��oU���7��k��f��������յ��[k}�(�?^?ϊ��(>Y���G[4��|i��"(�vA�������׿R�O?�v���-zK�C��1NyW�{����۷�����>J���P�}��7K}kg��p�T�K�␦����_ځ׷��w�2���l8߀�����#ZU�_v~���gnA'm����.���!�����ngF$4���ey����n!즨�6�������������5����y���ه��*��-p��L�F�g]�Kd�KI�/����[ܖq�P{Sj��������O�.�����+Ko������gTBԪ�
                                                                            u�D�7쿹���[쁱�X�J�r�!��@?����/K�T�U ��C�'��D�s�

   ���|
       �wfРK[��s�9�4
xa��Κ���6w����n@��@'�r�N�Qt(�>WϹ�25��X��)�ΆLP�|�~���>S<{�_�������-/�
                                                                    /
��>tP/$G���Sh��6�y�!����f��d>�Ȉ����z�<~S�NMS|��NY����L<'�g_�����@�}���S��K�6Ь�́��V����;sd��%JmOE)O�
                 �-�Ǥ���f~���n�!~�ő����N�v%�ٙ{^�d���|�z˙�30�"�`���+s"摁+O�mq.N�oҟzF?���gųw~�5�ΦO�Z��-Y��d<��!��Ϧn�U�~N�tfHpK^����Ly��3� ���yO��>�␦y��
�Wv@�-hǠ�\p8[����0��]F��Hi�ja�Z��[קع�w��6��ȓΑ��S�K˨tf�3���
                                  /C\��d�,ި�
�e�22:�
       ���?ɐ_�-$������'�4������/�̟̕\^�:
��b�'�H�J������/>��"/��IN��`Ӂ�K���B/[d�����[҈�
                                              �����|�.b[��e���^�Dw5I�<�sJg���LYB��C��
���Nl��"�VĈ����9EL����[{/t�E�YwCZ��~p��ĩ<_��vAv��/F[$�]��/Ur\KmX��Sn����\9�6)���r;�&i�/���H���[F'U�`�>;���HWO�y��
F'��Y�r�,��V�_Tƾ��o%�e��5��n���;6r���(y�Cd�����<d����&]32���BeM���4w��ed��@�;��z���:M�>]D���9.BƑ|dc�
                        Mf�h�8}�w~�&n�����&:?"קk!�>X�uO�-�8ʆܮ!�ɻ��k�O��9t{��n�3(�6����L]
�s;��xf`Gn;}�j�х��E     ��X;�;v��Б�M                                                    Qm�J�؏�f�ڑ�yD|>e��n'%EB���� �|
ȟ6�l�1x��␦f�Z�
              ���
                 ��
2=Ե�B   ��`�˞څ�C+�0֔H���e��������GV&��tC���ݠ_�(��^xHH�6�,�
                                                          d꺉��/�W��E��Փ�����N_�¿Iݳ��D_3<�[���G�a�C�͊�-�=r�O}����[�/-|>[��Y�v
      ��#�J�ʬR�Ǎ@��У�Y%����uk9W�
                                ������w�ή.��W��o��_��U�*��Hif��
                                                               ������2�"7��M��E,|�T���C����dVIчd�M&�Cø����==r4�b��eT��S
                                                                                                                       �|��΁c�$E���#dt�<�F~y����n�/����\�-��`e�%;���ʦd�$��*�Q�r:�γ���a���q�)�ab��;c%��pG �t�Ņ*�'�a~�
~����� Mu�C����2O       d @�t��#Zx9"�ЂO/QΌ��������A���}&ME�i�pr��� S���+�P
␦�-���,�/;{ [�lU=y|���/�D����*�I��2���7Pj��˸���
root@bioinfo_docker:/home/test/mapping# samtools-1.19.2/samtools sort THA2-bwa.bam -o THA2-bwa.sorted.bam
root@bioinfo_docker:/home/test/mapping# samtools-1.19.2/samtools index THA2-bwa.sorted.bam
root@bioinfo_docker:/home/test/mapping# cp THA2-bwa.sorted.bam THA2-bwa.sorted.bam.bai ../share
```

不知道为什么bowtie得到的结果导入IGV中没有任何显示
选择了NOT5基因展示
![作业4截图2](https://github.com/user-attachments/assets/f209b20c-81c6-4e75-a071-5da582d42ea6)


