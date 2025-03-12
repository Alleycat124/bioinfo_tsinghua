# Part I

## Code
```bash
#!/bin/bash
filefolder="bash_homework/"
files_output="filenames.txt"
dirs_output="dirname.txt"
> "$files_output"
> "$dirs_output"

for val in "$filefolder"*;
do
    if [ -f "$val" ]; then
        echo "$(basename "$val")" >> "$files_output"
    elif [ -d "$val" ]; then
        echo "$(basename "$val")" >> "$dirs_output"
    fi
done

echo "Files and directories have been written"
```

## Result
### filenames.txt
a1.txt  
a.txt  
b1.txt  
bam_wig.sh  
b.filter_random.pl  
c1.txt  
chrom.size  
c.txt  
d1.txt  
dir.txt  
e1.txt  
f1.txt  
human_geneExp.txt  
if.sh  
image  
insitiue.txt  
mouse_geneExp.txt  
name.txt  
number.sh  
out.bw  
random.sh  
read.sh  
test3.sh  
test4.sh  
test.sh  
test.txt  
wigToBigWig  


### dirname.txt
a-docker  
app  
backup  
bin  
biosoft  
c1-RBPanno  
datatable  
db  
download  
e-annotation  
exRNA  
genome  
git  
highcharts  
home  
hub29  
ibme  
l-lwl  
map2  
mljs  
module  
mogproject  
node_modules  
perl5  
postar2  
postar_app  
postar.docker  
RBP_map  
rout  
script  
script_backup  
software  
tcga  
test  
tmp  
tmp_script  
var  
x-rbp  



# Part II

## 1)

![作业3截图1](https://github.com/user-attachments/assets/5283ca33-474e-4fb6-a15b-5cbd6d215cc3 "作业截图1")
![作业3截图2](https://github.com/user-attachments/assets/dc8f2769-09e0-4cbd-b36b-db9bf507bf3a "作业截图2")
![作业3截图3](https://github.com/user-attachments/assets/c83be520-cd87-4390-a123-23eea26243df "作业截图3")



### E 值（Expect Value）：

E 值是随机匹配的期望数量，它表明在给定查询序列长度和数据库大小条件下，期望观测到的随机匹配的数量。E 值越小，说明这个比对的结果越显著，通常 E 值小于 0.05 或 0.1 被认为是有意义的。


### P 值（Probability Value）：

P 值表示假设检验中观察到的数据或更极端的数据在零假设下所发生的概率。在生物信息学中，P 值用于表示比对的显著性，其值越小，意味着结果越显著。具体来说，P值越小，代表在H0为真的前提下，获得观察结果的概率越小，那么这样从客观侧面也就是告诉我们可能需要否定H0假设而去接受H1
P 值通常与 E 值有密切的关系，通常 E 值为 0.01 时，对应的 P 值也会很小，反映出结果的显著性。


## 2)
### Code
```bash
#!/bin/bash  

# 给定的蛋白质序列  
original_sequence="MSTRSVSSSSYRRMFGGPGTASRPSSSRSYVTTSTRTYSLGSALRPSTSRSLYASSPGGVYATRSSAVRL"  

# 创建工作目录  
work_dir="blast_workdir"  
mkdir -p "$work_dir"  
cd "$work_dir"  

# 函数：随机打乱序列  
shuffle_sequence() {  
    echo "$1" | grep -o . | shuf | tr -d "\n"  
}  

# 生成 10 个随机打乱的序列并保存到文件  
for i in {1..10}; do  
    shuffled_sequence=$(shuffle_sequence "$original_sequence") 
    echo $shuffled_sequence #检查是否成功打乱
    echo ">shuffled_sequence_$i" > "sequence_$i.fasta"  
    echo "$shuffled_sequence" >> "sequence_$i.fasta"  
done  

# 创建 BLAST 数据库  
#makeblastdb -in "sequence_1.fasta" -dbtype prot -out my_blast_db  

# 对所有序列进行两两 BLAST 比对  
results_file="blast_results.txt"  
#echo "Query Sequence, Subject Sequence, % Identity, Alignment Length, Mismatches, Gap Opens, Gaps, Q. Start, Q. End, S. Start, S. End, E-value, Bit Score" > "$results_file"  

for query in sequence_*.fasta; do  
    for subject in sequence_*.fasta; do  
        # 确保查询与目标序列不同，避免自我比对  
        if [ "$query" != "$subject" ]; then  
            echo $query $subject
            blastp -query "$query" -subject "$subject" -outfmt 6  >> "$results_file"  
            #"6 qseqid sseqid pident length mismatch gapopen gaps qstart qend sstart send evalue bitscore"
        fi  
    done  
done  

echo "BLAST 比对完成，结果已保存到 $work_dir/$results_file"  
```

### Result（举一个例子）
Query Sequence, Subject Sequence, % Identity, Alignment Length, Mismatches, Gap Opens, Gaps, Q. Start, Q. End, S. Start, S. End, E-value, Bit Score
shuffled_sequence_10	shuffled_sequence_2	50.000	16	8	0	0	31	46	10	25	0.18	13.9


Query Sequence：查询序列的名称。
Subject Sequence：比对对象序列的名称。
% Identity：两个序列之间的序列相似性的百分比。
Alignment Length：比对中比对的序列的长度。
Mismatches：比对中发现的不匹配的氨基酸位点数。
Gap Opens：比对中出现的 gaps 的数量。
Gaps：序列中存在的 gaps 的总数，即插入或缺失的情况。比如如果在一个比对中有一个缺口长度为 3 的区域，那么这个缺口会被计算为一个 gap open，但在计算“gaps”时，它会被记录为 3。
Q. Start 和 Q. End：查询序列中比对开始和结束的位置。
S. Start 和 S. End：比对对象序列中比对开始和结束的位置。
E-value：期望值，表示在数据库中随机找到相同或更好的比对的期望数，值越小表示比对结果越显著。
Bit Score：比对的得分，分数越高表示比对的相关性越强

## 3)
### 索引和预先计算的数据库：

目标： BLAST 使用了预先构建的数据库索引，以快速检索潜在的匹配序列。

实现方法：通过使用短的、高频的 k-mer（例如，词典或模式），BLAST 首先迅速查找与查询序列中相符的 k-mer。这一步骤可以显著减少需要进行完整比对的序列数量。

### 分块比对（Seed-and-extend）：

目标：BLAST 的核心思想是分块比对，即先进行快速的种子（seed）匹配，然后再进行扩展确认。

实现方法：BLAST 首先找到查询和库序列中的涉及到的短高匹配片段（种子阶段），然后通过动态规划在局部进行细化。这使得 BLAST 可以在巨大数据库中迅速识别出潜在的良好比对，避免了处理整个序列的时间损耗。



## 4)
### 对称 PAM250 矩阵

定义：
对称 PAM250 矩阵中，氨基酸替换的得分是基于相同的替换概率，因此矩阵中 a→b 的得分和 b→a 的得分是相等的。

特征：
这种形式通常用于简化情况，适用于所有氨基酸对之间等价的比较。
在进行局部或全局比对时，使用对称矩阵可以保持一致性，方便计算。

应用：
多用于广泛的序列比对，特别是在研究毒性、折叠及功能相关性等场合，适合对任意两个序列间进行全局比较。

### 不对称 PAM250 矩阵

定义：
不对称 PAM250 矩阵则允许特定氨基酸的替代得分不等，具体取决于生物学上的替代方向。例如，从氨基酸 A 替换为氨基酸 B 和从 B 替换为 A 的得分可能不同。

特征：
这种形式可以根据不同的生物学背景或进化压力对序列的突变做出更真实的反应。
它能够捕捉更复杂的生物学信号，例如不同氨基酸在突变中具有的环境适应性或功能需求。

应用：
不对称 PAM250 通常适用于特定研究需求，尤其是当对替换方向有明确生物学意义时，如某些酶的催化过程或蛋白质交互作用。
适用于需要考虑氨基酸替换方向性而导致不同生物学效应的分析，比如进化生物学或比较基因组学。

