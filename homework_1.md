```bash
C:\Users\18417>
C:\Users\18417>docker exec -it KunlinDu_Linux bash
test@bioinfo_docker:~$ ls
alter-spl  bwa.git   diff-exp  homer  mapping  sacCer3.fa         share    software
blast      chip-seq  gsea      linux  plot     samtools_bedtools  sharing
test@bioinfo_docker:~$ 本次代码是我（杜昆霖）完成的，我没有复制任何来源的代码
bash: 本次代码是我（杜昆霖）完成的，我没有复制任何来源的代码: command not found
test@bioinfo_docker:~$ ls -a
.              .bashrc   gsea     plot               sharing
..             blast     homer    .profile           software
alter-spl      bwa.git   linux    sacCer3.fa         .vim
.bash_history  chip-seq  .local   samtools_bedtools  .viminfo
.bash_logout   diff-exp  mapping  share              .wget-hsts
test@bioinfo_docker:~$ ls -l /bin
total 5152
-rwxr-xr-x 1 root root 1113504 Apr  4  2018 bash
-rwxr-xr-x 3 root root   34888 Jan 29  2017 bunzip2
-rwxr-xr-x 3 root root   34888 Jan 29  2017 bzcat
lrwxrwxrwx 1 root root       6 Jan 29  2017 bzcmp -> bzdiff
-rwxr-xr-x 1 root root    2140 Jan 29  2017 bzdiff
lrwxrwxrwx 1 root root       6 Jan 29  2017 bzegrep -> bzgrep
-rwxr-xr-x 1 root root    4877 Jan 29  2017 bzexe
lrwxrwxrwx 1 root root       6 Jan 29  2017 bzfgrep -> bzgrep
-rwxr-xr-x 1 root root    3642 Jan 29  2017 bzgrep
-rwxr-xr-x 3 root root   34888 Jan 29  2017 bzip2
-rwxr-xr-x 1 root root   14328 Jan 29  2017 bzip2recover
lrwxrwxrwx 1 root root       6 Jan 29  2017 bzless -> bzmore
-rwxr-xr-x 1 root root    1297 Jan 29  2017 bzmore
-rwxr-xr-x 1 root root   35064 Jan 18  2018 cat
-rwxr-xr-x 1 root root   63672 Jan 18  2018 chgrp
-rwxr-xr-x 1 root root   59608 Jan 18  2018 chmod
-rwxr-xr-x 1 root root   67768 Jan 18  2018 chown
-rwxr-xr-x 1 root root  141528 Jan 18  2018 cp
-rwxr-xr-x 1 root root  121432 Jan 25  2018 dash
-rwxr-xr-x 1 root root  100568 Jan 18  2018 date
-rwxr-xr-x 1 root root   76000 Jan 18  2018 dd
-rwxr-xr-x 1 root root   84776 Jan 18  2018 df
-rwxr-xr-x 1 root root  133792 Jan 18  2018 dir
-rwxr-xr-x 1 root root   72000 May 16  2018 dmesg
lrwxrwxrwx 1 root root       8 Jan 31  2018 dnsdomainname -> hostname
lrwxrwxrwx 1 root root       8 Jan 31  2018 domainname -> hostname
-rwxr-xr-x 1 root root   35000 Jan 18  2018 echo
-rwxr-xr-x 1 root root      28 Jul 12  2017 egrep
-rwxr-xr-x 1 root root   30904 Jan 18  2018 false
-rwxr-xr-x 1 root root      28 Jul 12  2017 fgrep
-rwxr-xr-x 1 root root   64784 May 16  2018 findmnt
-rwxr-xr-x 1 root root  219528 Jul 12  2017 grep
-rwxr-xr-x 2 root root    2301 Apr 28  2017 gunzip
-rwxr-xr-x 1 root root    5927 Apr 28  2017 gzexe
-rwxr-xr-x 1 root root  101560 Apr 28  2017 gzip
-rwxr-xr-x 1 root root   18504 Jan 31  2018 hostname
-rwxr-xr-x 1 root root   26704 May 14  2018 kill
-rwxr-xr-x 1 root root  170760 Dec  1  2017 less
-rwxr-xr-x 1 root root   10256 Dec  1  2017 lessecho
lrwxrwxrwx 1 root root       8 Dec  1  2017 lessfile -> lesspipe
-rwxr-xr-x 1 root root   19856 Dec  1  2017 lesskey
-rwxr-xr-x 1 root root    8564 Dec  1  2017 lesspipe
-rwxr-xr-x 1 root root   67808 Jan 18  2018 ln
-rwxr-xr-x 1 root root   52664 Jan 25  2018 login
-rwxr-xr-x 1 root root  133792 Jan 18  2018 ls
-rwxr-xr-x 1 root root   84048 May 16  2018 lsblk
-rwxr-xr-x 1 root root   80056 Jan 18  2018 mkdir
-rwxr-xr-x 1 root root   67768 Jan 18  2018 mknod
-rwxr-xr-x 1 root root   43192 Jan 18  2018 mktemp
-rwxr-xr-x 1 root root   38952 May 16  2018 more
-rwsr-xr-x 1 root root   43088 May 16  2018 mount
-rwxr-xr-x 1 root root   14408 May 16  2018 mountpoint
-rwxr-xr-x 1 root root  137440 Jan 18  2018 mv
lrwxrwxrwx 1 root root       8 Jan 31  2018 nisdomainname -> hostname
lrwxrwxrwx 1 root root      14 Nov  1  2017 pidof -> /sbin/killall5
-rwxr-xr-x 1 root root  133432 May 14  2018 ps
-rwxr-xr-x 1 root root   35000 Jan 18  2018 pwd
lrwxrwxrwx 1 root root       4 Apr  4  2018 rbash -> bash
-rwxr-xr-x 1 root root   43192 Jan 18  2018 readlink
-rwxr-xr-x 1 root root   63704 Jan 18  2018 rm
-rwxr-xr-x 1 root root   43192 Jan 18  2018 rmdir
-rwxr-xr-x 1 root root   18760 Dec 30  2017 run-parts
-rwxr-xr-x 1 root root  109000 Jan 30  2018 sed
lrwxrwxrwx 1 root root       4 Sep 11  2018 sh -> dash
lrwxrwxrwx 1 root root       4 Aug 21  2018 sh.distrib -> dash
-rwxr-xr-x 1 root root   35000 Jan 18  2018 sleep
-rwxr-xr-x 1 root root   75992 Jan 18  2018 stty
-rwsr-xr-x 1 root root   44664 Jan 25  2018 su
-rwxr-xr-x 1 root root   35000 Jan 18  2018 sync
-rwxr-xr-x 1 root root  423384 Jul 21  2017 tar
-rwxr-xr-x 1 root root   10104 Dec 30  2017 tempfile
-rwxr-xr-x 1 root root   88280 Jan 18  2018 touch
-rwxr-xr-x 1 root root   30904 Jan 18  2018 true
-rwsr-xr-x 1 root root   26696 May 16  2018 umount
-rwxr-xr-x 1 root root   35032 Jan 18  2018 uname
-rwxr-xr-x 2 root root    2301 Apr 28  2017 uncompress
-rwxr-xr-x 1 root root  133792 Jan 18  2018 vdir
-rwxr-xr-x 1 root root   30800 May 16  2018 wdctl
-rwxr-xr-x 1 root root     946 Dec 30  2017 which
lrwxrwxrwx 1 root root       8 Jan 31  2018 ypdomainname -> hostname
-rwxr-xr-x 1 root root    1937 Apr 28  2017 zcat
-rwxr-xr-x 1 root root    1777 Apr 28  2017 zcmp
-rwxr-xr-x 1 root root    5764 Apr 28  2017 zdiff
-rwxr-xr-x 1 root root     140 Apr 28  2017 zegrep
-rwxr-xr-x 1 root root     140 Apr 28  2017 zfgrep
-rwxr-xr-x 1 root root    2131 Apr 28  2017 zforce
-rwxr-xr-x 1 root root    5938 Apr 28  2017 zgrep
-rwxr-xr-x 1 root root    2037 Apr 28  2017 zless
-rwxr-xr-x 1 root root    1910 Apr 28  2017 zmore
-rwxr-xr-x 1 root root    5047 Apr 28  2017 znew
test@bioinfo_docker:~$ ls
alter-spl  bwa.git   diff-exp  homer  mapping  sacCer3.fa         share    software
blast      chip-seq  gsea      linux  plot     samtools_bedtools  sharing
test@bioinfo_docker:~$ mkdir practice
test@bioinfo_docker:~$ rm practice
rm: cannot remove 'practice': Is a directory
test@bioinfo_docker:~$ rmdir practice
test@bioinfo_docker:~$ mkdir test
test@bioinfo_docker:~$ cd test
test@bioinfo_docker:~/test$ tar -c -v -f archive.tar main.c main.h
tar: main.c: Cannot stat: No such file or directory
tar: main.h: Cannot stat: No such file or directory
tar: Exiting with failure status due to previous errors
test@bioinfo_docker:~/test$ touch main.c main.h
test@bioinfo_docker:~/test$ tar -c -v -f archive.tar main.c main.h
main.c
main.h
test@bioinfo_docker:~/test$ ls -l
total 12
-rw-r--r-- 1 test test 10240 Sep 25 05:03 archive.tar
-rw-r--r-- 1 test test     0 Sep 25 05:03 main.c
-rw-r--r-- 1 test test     0 Sep 25 05:03 main.h
test@bioinfo_docker:~/test$ echo hello world
hello world
test@bioinfo_docker:~/test$ passwd
Changing password for test.
(current) UNIX password:



passwd: Authentication token manipulation error
passwd: password unchanged
test@bioinfo_docker:~/test$
test@bioinfo_docker:~/test$
test@bioinfo_docker:~/test$
test@bioinfo_docker:~/test$ date
Thu Sep 25 05:09:28 UTC 2025
test@bioinfo_docker:~/test$ hostname
bioinfo_docker
test@bioinfo_docker:~/test$ arch
x86_64
test@bioinfo_docker:~/test$ uname -a
Linux bioinfo_docker 5.15.167.4-microsoft-standard-WSL2 #1 SMP Tue Nov 5 00:21:55 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux
test@bioinfo_docker:~/test$
```
