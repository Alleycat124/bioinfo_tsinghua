## 1
**input DNA** 指的是在CHIP-seq过程中不用任何抗体捕获的DNA，即破碎产物直接解交联，纯化的DNA  
通过Input对照可以排除因本底表达水平高或一些非特异性结合所造成的假阳性peaks。

## 2
**findPeaks**  
-style: 有factor,histone两种选择。如前所述，转录因子和组蛋白修饰的CHIP-seq peak有不同的特性，所以在peak calling中也会使用不同的策略。

-o : 输出文件路径

-i : 存储input样本中间文件的"tag directory


**findMotifsGenome**  
-size: 设置寻找motif的区域大小，默认值为200。

-len: 设置motif的长度，默认值为8,10,12。

-bg: 自定义背景序列文件。

-mis: 允许的错配碱基数，默认值为2。

-S: 输出的motif数量，默认值为25。

-norevopp: 不进行反义链搜索motif。

-rna: 输出RNA motif，使用RNA motif数据库。

## 3




![image](https://github.com/user-attachments/assets/e2fcb771-8de8-4da7-9816-a83ad26ba0dc)


![作业9截图](https://github.com/user-attachments/assets/fac4375f-197d-4c88-bdf0-7f02004a1052)





