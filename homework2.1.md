```bash
test@bioinfo_docker:~/share$ wc -l test_command.gtf
8 test_command.gtf
test@bioinfo_docker:~/share$ wc -c test_command.gtf
636 test_command.gtf
test@bioinfo_docker:~/share$ wc test_command.gtf
  8  96 636 test_command.gtf
test@bioinfo_docker:~/share$ wc -l -c -m test_command.gtf
  8 636 636 test_command.gtf


test@bioinfo_docker:~/share$ grep "^chr_" test_command.gtf | grep -w "YDL248W"
chr_IV  ensembl gene    1802    2953    .       +       .       gene_id "YDL248W"; gene_version "1";
chr_IV  ensembl transcript      802     2953    .       +       .       gene_id "YDL248W"; gene_version "1";
chr_IV  ensembl start_codon     1802    1804    .       +       0       gene_id "YDL248W"; gene_version "1";


test@bioinfo_docker:~/share$ sed 's/chr_/chromosome_/g' test_command.gtf | awk '{print $1, $3, $4, $5}'
chromosome_IV gene 1802 2953
chromosome_IV transcript 802 2953
chromosome_IV exon 1802 2953
chromosome_IV CDS 1802 950
chromosome_IV start_codon 1802 1804
chromosome_IV stop_codon 2951 2953
chromosome_IV gene 762 3836
chromosome_IV transcript 3762 836
test@bioinfo_docker:~/share$ sed 's/chr_/chromosome_/g' test_command.gtf | cut -f 1,3-5
chromosome_IV   gene    1802    2953
chromosome_IV   transcript      802     2953
chromosome_IV   exon    1802    2953
chromosome_IV   CDS     1802    950
chromosome_IV   start_codon     1802    1804
chromosome_IV   stop_codon      2951    2953
chromosome_IV   gene    762     3836
chromosome_IV   transcript      3762    836


test@bioinfo_docker:~/share$ awk -v OFS='\t' '{temp=$2; $2=$3; $3=temp; print}' test_command.gtf | sort -k 4n -k 5n > result.gtf
test@bioinfo_docker:~/share$ cat result.gtf
chromosome_IV   gene    ensembl 762     3836    .       +       .       gene_id "YDL247W-A";    gene_version    "1";
chr_IV  transcript      ensembl 802     2953    .       +       .       gene_id "YDL248W";      gene_version    "1";
chromosome_IV   CDS     ensembl 1802    950     .       +       0       gene_id "YDL248W";      gene_version    "1";
chr_IV  start_codon     ensembl 1802    1804    .       +       0       gene_id "YDL248W";      gene_version    "1";
chr_IV  gene    ensembl 1802    2953    .       +       .       gene_id "YDL248W";      gene_version    "1";
chromosome_IV   exon    ensembl 1802    2953    .       +       .       gene_id "YDL248W";      gene_version    "1";
chromosome_IV   stop_codon      ensembl 2951    2953    .       +       0       gene_id "YDL248W";      gene_version   "1";
chr_IV  transcript      ensembl 3762    836     .       +       .       gene_id "YDL247W-A";    gene_version    "1";


test@bioinfo_docker:~/share$ ls -hl test_command.gtf
'''
-rwxrwxrwx 1 test test 636 Mar  4 07:05 test_command.gtf
test@bioinfo_docker:~/share$ chmod 774 test_command.gtf
test@bioinfo_docker:~/share$ ls -hl test_command.gtf
-rwxrwxr-- 1 test test 636 Mar  4 07:05 test_command.gtf
