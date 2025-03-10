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

# filenames.txt
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


# dirname.txt
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
