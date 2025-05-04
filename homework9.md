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

```bash
test@bioinfo_docker:~$ cd chip-seq/
test@bioinfo_docker:~/chip-seq$ ls
homework  input  output  output_homework
test@bioinfo_docker:~/chip-seq$ cd homework
test@bioinfo_docker:~/chip-seq/homework$ ls -la
total 85024
drwxr-xr-x 1 test test     4096 Dec  1  2019 .
drwxr-xr-x 1 test test     4096 Dec  1  2019 ..
drwxr-xr-x 2 test test     4096 Dec  1  2019 input
-r--r--r-- 1 test test 48128551 Sep 11  2018 input.chrom_part.bam
drwxr-xr-x 2 test test     4096 Dec  1  2019 ip
-r--r--r-- 1 test test 38913836 Sep 11  2018 ip.chrom_part.bam
test@bioinfo_docker:~/chip-seq/homework$ findPeaks ip
ip/                ip.chrom_part.bam
test@bioinfo_docker:~/chip-seq/homework$ cd ip
test@bioinfo_docker:~/chip-seq/homework/ip$ ls
chrIV.tags.tsv  tagAutocorrelation.txt    tagInfo.txt
chrV.tags.tsv   tagCountDistribution.txt  tagLengthDistribution.txt
test@bioinfo_docker:~/chip-seq/homework/ip$ cd ..
test@bioinfo_docker:~/chip-seq/homework$ findPeaks ip/ -style factor -o output/part.peak -i input/
        Fragment Length = 233
Could not open output/part.peak for writing!!!
test@bioinfo_docker:~/chip-seq/homework$ mkdir output
test@bioinfo_docker:~/chip-seq/homework$ findPeaks ip/ -style factor -o output/part.peak -i input/
        Fragment Length = 233
        !!! Estimated genome size (from tag directory) is smaller than default
            genome size.  Using estimate (2097191) [to change specify -gsize]
        Total Tags = 1402413.0
        Tags per bp = 0.668710
        Max tags per bp set automatically to 66.0
        Finding peaks of size 248, no closer than 496
                Finding peaks on chrIV (minCount=164.8), total tags positions = 706357
                Finding peaks on chrV (minCount=164.8), total tags positions = 294957
                Tags Used for cluster (less clonal tags) = 1402326.0 / 1402413.0
        Expected tags per peak = 165.829834 (tbp = 0.668669)
                Threshold       Peak Count      Expected Peak Count     FDR     Poisson
                247     311.000 0.000   1.33e-07        2.45e-09
                246     312.000 0.000   2.00e-07        3.68e-09
                245     318.000 0.000   2.93e-07        5.51e-09
                244     320.000 0.000   4.33e-07        8.20e-09
                243     325.000 0.000   6.33e-07        1.22e-08
                242     328.000 0.000   9.27e-07        1.80e-08
                241     333.000 0.000   1.34e-06        2.65e-08
                240     334.000 0.001   1.96e-06        3.88e-08
                239     335.000 0.001   2.86e-06        5.66e-08
                238     338.000 0.001   4.12e-06        8.23e-08
                237     342.000 0.002   5.89e-06        1.19e-07
                236     345.000 0.003   8.43e-06        1.72e-07
                235     347.000 0.004   1.20e-05        2.47e-07
                234     352.000 0.006   1.70e-05        3.53e-07
                233     354.000 0.009   2.40e-05        5.03e-07
                232     355.000 0.012   3.40e-05        7.14e-07
                231     359.000 0.017   4.75e-05        1.01e-06
                230     362.000 0.024   6.63e-05        1.42e-06
                229     366.000 0.034   9.19e-05        1.99e-06
                228     369.000 0.047   1.27e-04        2.78e-06
                227     372.000 0.065   1.75e-04        3.86e-06
                226     379.000 0.090   2.38e-04        5.34e-06
                225     379.000 0.124   3.28e-04        7.36e-06
                224     387.000 0.171   4.41e-04        1.01e-05
                223     395.000 0.233   5.91e-04        1.38e-05
                222     402.000 0.317   7.90e-04        1.88e-05
                221     406.000 0.430   1.06e-03        2.54e-05
                220     408.000 0.580   1.42e-03        3.43e-05
                219     413.000 0.779   1.89e-03        4.61e-05
                218     420.000 1.042   2.48e-03        6.16e-05
                217     427.000 1.388   3.25e-03        8.21e-05
                216     436.000 1.840   4.22e-03        1.09e-04
                215     441.000 2.429   5.51e-03        1.44e-04
                214     445.000 3.193   7.18e-03        1.89e-04
                213     450.000 4.179   9.29e-03        2.47e-04
                212     456.000 5.445   1.19e-02        3.22e-04
                211     462.000 7.064   1.53e-02        4.18e-04
                210     470.000 9.124   1.94e-02        5.39e-04
                209     481.000 11.732  2.44e-02        6.94e-04
                208     491.000 15.020  3.06e-02        8.88e-04
                207     495.000 19.143  3.87e-02        1.13e-03
                206     506.000 24.290  4.80e-02        1.44e-03
                205     518.000 30.684  5.92e-02        1.81e-03
                204     525.000 38.588  7.35e-02        2.28e-03
                203     535.000 48.312  9.03e-02        2.86e-03
                202     542.000 60.214  1.11e-01        3.56e-03
                201     548.000 74.714  1.36e-01        4.42e-03
                200     556.000 92.288  1.66e-01        5.46e-03
                199     568.000 113.484 2.00e-01        6.71e-03
                198     585.000 138.919 2.37e-01        8.21e-03
                197     593.000 169.288 2.85e-01        1.00e-02
                196     608.000 205.366 3.38e-01        1.21e-02
                195     616.000 248.008 4.03e-01        1.47e-02
                194     631.000 298.152 4.73e-01        1.76e-02
                193     642.000 356.811 5.56e-01        2.11e-02
                192     659.000 425.082 6.45e-01        2.51e-02
                191     672.000 504.128 7.50e-01        2.98e-02
                190     682.000 595.171 8.73e-01        3.52e-02
                189     697.000 699.482 1.00e+00        4.14e-02
                188     710.000 818.369 1.00e+00        4.84e-02
                187     724.000 953.151 1.00e+00        5.64e-02
                186     743.000 1105.145        1.00e+00        6.53e-02
                185     750.000 1275.619        1.00e+00        7.54e-02
                184     767.000 1465.808        1.00e+00        8.67e-02
                183     787.000 1676.830        1.00e+00        9.91e-02
                182     797.000 1909.699        1.00e+00        1.13e-01
                181     815.000 2165.283        1.00e+00        1.28e-01
                180     827.000 2444.245        1.00e+00        1.45e-01
                179     842.000 2747.037        1.00e+00        1.62e-01
                178     858.000 3073.874        1.00e+00        1.82e-01
                177     869.000 3424.711        1.00e+00        2.02e-01
                176     884.000 3799.180        1.00e+00        2.25e-01
                175     906.000 4196.609        1.00e+00        2.48e-01
                174     922.000 4616.020        1.00e+00        2.73e-01
                173     938.000 5056.095        1.00e+00        2.99e-01
                172     953.000 5515.184        1.00e+00        3.26e-01
                171     979.000 5991.367        1.00e+00        3.54e-01
                170     1001.000        6482.396        1.00e+00        3.83e-01
                169     1020.000        6985.775        1.00e+00        4.13e-01
                168     1030.000        7498.769        1.00e+00        4.43e-01
                167     1043.000        8018.476        1.00e+00        4.74e-01
                166     1059.000        8541.844        1.00e+00        5.05e-01
                165     1073.000        9065.754        1.00e+00        5.36e-01
                164     1073.000        9587.049        1.00e+00        5.67e-01
                163     1073.000        10102.585       1.00e+00        5.97e-01
                162     1073.000        10609.324       1.00e+00        6.27e-01
                161     1073.000        11104.354       1.00e+00        6.57e-01
                160     1073.000        11584.974       1.00e+00        6.85e-01
                159     1073.000        12048.707       1.00e+00        7.12e-01
                158     1073.000        12493.344       1.00e+00        7.39e-01
                157     1073.000        12916.972       1.00e+00        7.64e-01
                156     1073.000        13318.055       1.00e+00        7.87e-01
                155     1073.000        13695.366       1.00e+00        8.10e-01
                154     1073.000        14048.027       1.00e+00        8.31e-01
                153     1073.000        14375.540       1.00e+00        8.50e-01
                152     1073.000        14677.703       1.00e+00        8.68e-01
                151     1073.000        14954.661       1.00e+00        8.84e-01
                150     1073.000        15206.865       1.00e+00        8.99e-01
                149     1073.000        15434.990       1.00e+00        9.13e-01
                148     1073.000        15639.955       1.00e+00        9.25e-01
                147     1073.000        15822.891       1.00e+00        9.36e-01
                146     1073.000        15985.051       1.00e+00        9.45e-01
                145     1073.000        16127.817       1.00e+00        9.54e-01
                144     1073.000        16252.652       1.00e+00        9.61e-01
                143     1073.000        16361.057       1.00e+00        9.67e-01
                142     1073.000        16454.536       1.00e+00        9.73e-01
                141     1073.000        16534.582       1.00e+00        9.78e-01
                140     1073.000        16602.644       1.00e+00        9.82e-01
                139     1073.000        16660.105       1.00e+00        9.85e-01
                138     1073.000        16708.268       1.00e+00        9.88e-01
                137     1073.000        16748.348       1.00e+00        9.90e-01
                136     1073.000        16781.460       1.00e+00        9.92e-01
                135     1073.000        16808.616       1.00e+00        9.94e-01
                134     1073.000        16830.724       1.00e+00        9.95e-01
                133     1073.000        16848.587       1.00e+00        9.96e-01
                132     1073.000        16862.915       1.00e+00        9.97e-01
                131     1073.000        16874.320       1.00e+00        9.98e-01
                130     1073.000        16883.329       1.00e+00        9.98e-01
                129     1073.000        16890.392       1.00e+00        9.99e-01
                128     1073.000        16895.886       1.00e+00        9.99e-01
                127     1073.000        16900.126       1.00e+00        9.99e-01
                126     1073.000        16903.374       1.00e+00        9.99e-01
                125     1073.000        16905.842       1.00e+00        1.00e+00
                124     1073.000        16907.702       1.00e+00        1.00e+00
                123     1073.000        16909.093       1.00e+00        1.00e+00
                122     1073.000        16910.125       1.00e+00        1.00e+00
                121     1073.000        16910.884       1.00e+00        1.00e+00
                120     1073.000        16911.437       1.00e+00        1.00e+00
                119     1073.000        16911.838       1.00e+00        1.00e+00
                118     1073.000        16912.126       1.00e+00        1.00e+00
                117     1073.000        16912.330       1.00e+00        1.00e+00
                116     1073.000        16912.475       1.00e+00        1.00e+00
                115     1073.000        16912.576       1.00e+00        1.00e+00
                114     1073.000        16912.646       1.00e+00        1.00e+00
                113     1073.000        16912.694       1.00e+00        1.00e+00
                112     1073.000        16912.727       1.00e+00        1.00e+00
                111     1073.000        16912.749       1.00e+00        1.00e+00
                110     1073.000        16912.764       1.00e+00        1.00e+00
                109     1073.000        16912.774       1.00e+00        1.00e+00
                108     1073.000        16912.780       1.00e+00        1.00e+00
                107     1073.000        16912.784       1.00e+00        1.00e+00
                106     1073.000        16912.787       1.00e+00        1.00e+00
                105     1073.000        16912.789       1.00e+00        1.00e+00
                104     1073.000        16912.790       1.00e+00        1.00e+00
                103     1073.000        16912.790       1.00e+00        1.00e+00
                102     1073.000        16912.791       1.00e+00        1.00e+00
                101     1073.000        16912.791       1.00e+00        1.00e+00
                100     1073.000        16912.791       1.00e+00        1.00e+00
                99      1073.000        16912.791       1.00e+00        1.00e+00
                98      1073.000        16912.791       1.00e+00        1.00e+00
                97      1073.000        16912.791       1.00e+00        1.00e+00
                96      1073.000        16912.792       1.00e+00        1.00e+00
                95      1073.000        16912.792       1.00e+00        1.00e+00
                94      1073.000        16912.792       1.00e+00        1.00e+00
                93      1073.000        16912.792       1.00e+00        1.00e+00
                92      1073.000        16912.792       1.00e+00        1.00e+00
                91      1073.000        16912.792       1.00e+00        1.00e+00
                90      1073.000        16912.792       1.00e+00        1.00e+00
                89      1073.000        16912.792       1.00e+00        1.00e+00
                88      1073.000        16912.792       1.00e+00        1.00e+00
                87      1073.000        16912.792       1.00e+00        1.00e+00
                86      1073.000        16912.792       1.00e+00        1.00e+00
                85      1073.000        16912.792       1.00e+00        1.00e+00
                84      1073.000        16912.792       1.00e+00        1.00e+00
                83      1073.000        16912.792       1.00e+00        1.00e+00
                82      1073.000        16912.792       1.00e+00        1.00e+00
                81      1073.000        16912.792       1.00e+00        1.00e+00
                80      1073.000        16912.792       1.00e+00        1.00e+00
                79      1073.000        16912.792       1.00e+00        1.00e+00
                78      1073.000        16912.792       1.00e+00        1.00e+00
                77      1073.000        16912.792       1.00e+00        1.00e+00
                76      1073.000        16912.792       1.00e+00        1.00e+00
                75      1073.000        16912.792       1.00e+00        1.00e+00
                74      1073.000        16912.792       1.00e+00        1.00e+00
                73      1073.000        16912.792       1.00e+00        1.00e+00
                72      1073.000        16912.792       1.00e+00        1.00e+00
                71      1073.000        16912.792       1.00e+00        1.00e+00
                70      1073.000        16912.792       1.00e+00        1.00e+00
                69      1073.000        16912.792       1.00e+00        1.00e+00
                68      1073.000        16912.792       1.00e+00        1.00e+00
                67      1073.000        16912.792       1.00e+00        1.00e+00
                66      1073.000        16912.792       1.00e+00        1.00e+00
                65      1073.000        16912.792       1.00e+00        1.00e+00
                64      1073.000        16912.792       1.00e+00        1.00e+00
                63      1073.000        16912.792       1.00e+00        1.00e+00
                62      1073.000        16912.792       1.00e+00        1.00e+00
                61      1073.000        16912.792       1.00e+00        1.00e+00
                60      1073.000        16912.792       1.00e+00        1.00e+00
                59      1073.000        16912.792       1.00e+00        1.00e+00
                58      1073.000        16912.792       1.00e+00        1.00e+00
                57      1073.000        16912.792       1.00e+00        1.00e+00
                56      1073.000        16912.792       1.00e+00        1.00e+00
                55      1073.000        16912.792       1.00e+00        1.00e+00
                54      1073.000        16912.792       1.00e+00        1.00e+00
                53      1073.000        16912.792       1.00e+00        1.00e+00
                52      1073.000        16912.792       1.00e+00        1.00e+00
                51      1073.000        16912.792       1.00e+00        1.00e+00
                50      1073.000        16912.792       1.00e+00        1.00e+00
                49      1073.000        16912.792       1.00e+00        1.00e+00
                48      1073.000        16912.792       1.00e+00        1.00e+00
                47      1073.000        16912.792       1.00e+00        1.00e+00
                46      1073.000        16912.792       1.00e+00        1.00e+00
                45      1073.000        16912.792       1.00e+00        1.00e+00
                44      1073.000        16912.792       1.00e+00        1.00e+00
                43      1073.000        16912.792       1.00e+00        1.00e+00
                42      1073.000        16912.792       1.00e+00        1.00e+00
                41      1073.000        16912.792       1.00e+00        1.00e+00
                40      1073.000        16912.792       1.00e+00        1.00e+00
                39      1073.000        16912.792       1.00e+00        1.00e+00
                38      1073.000        16912.792       1.00e+00        1.00e+00
                37      1073.000        16912.792       1.00e+00        1.00e+00
                36      1073.000        16912.792       1.00e+00        1.00e+00
                35      1073.000        16912.792       1.00e+00        1.00e+00
                34      1073.000        16912.792       1.00e+00        1.00e+00
                33      1073.000        16912.792       1.00e+00        1.00e+00
                32      1073.000        16912.792       1.00e+00        1.00e+00
                31      1073.000        16912.792       1.00e+00        1.00e+00
                30      1073.000        16912.792       1.00e+00        1.00e+00
                29      1073.000        16912.792       1.00e+00        1.00e+00
                28      1073.000        16912.792       1.00e+00        1.00e+00
                27      1073.000        16912.792       1.00e+00        1.00e+00
                26      1073.000        16912.792       1.00e+00        1.00e+00
                25      1073.000        16912.792       1.00e+00        1.00e+00
                24      1073.000        16912.792       1.00e+00        1.00e+00
                23      1073.000        16912.792       1.00e+00        1.00e+00
                22      1073.000        16912.792       1.00e+00        1.00e+00
                21      1073.000        16912.792       1.00e+00        1.00e+00
                20      1073.000        16912.792       1.00e+00        1.00e+00
                19      1073.000        16912.792       1.00e+00        1.00e+00
                18      1073.000        16912.792       1.00e+00        1.00e+00
                17      1073.000        16912.792       1.00e+00        1.00e+00
                16      1073.000        16912.792       1.00e+00        1.00e+00
                15      1073.000        16912.792       1.00e+00        1.00e+00
                14      1073.000        16912.792       1.00e+00        1.00e+00
                13      1073.000        16912.792       1.00e+00        1.00e+00
                12      1073.000        16912.792       1.00e+00        1.00e+00
                11      1073.000        16912.792       1.00e+00        1.00e+00
                10      1073.000        16912.792       1.00e+00        1.00e+00
                9       1073.000        16912.792       1.00e+00        1.00e+00
                8       1073.000        16912.792       1.00e+00        1.00e+00
                7       1073.000        16912.792       1.00e+00        1.00e+00
                6       1073.000        16912.792       1.00e+00        1.00e+00
                5       1073.000        16912.792       1.00e+00        1.00e+00
                4       1073.000        16912.792       1.00e+00        1.00e+00
                3       1073.000        16912.792       1.00e+00        1.00e+00
                2       1073.000        16912.792       1.00e+00        1.00e+00
                1       1073.000        16912.792       1.00e+00        1.00e+00
                0       1073.000        16912.792       1.00e+00        1.00e+00
        0.10% FDR Threshold set at 222.0 (poisson pvalue ~ 1.88e-05)
        402 peaks passed threshold
        Differential Peaks: 79 of 402 (19.65% passed)
        Local Background Filtering: 69 of 79 (87.34% passed)
        Clonal filtering: 69 of 69 (100.00% passed)
        Total Peaks identified = 69
        Centering peaks of size 248 using a fragment length of 233
test@bioinfo_docker:~/chip-seq/homework$ findMotifsGenome.pl output/part.peak sacCer2 output/part.motif.output -len 8

        Position file = output/part.peak
        Genome = sacCer2
        Output Directory = output/part.motif.output
        Motif length set at 8,
        Found mset for "yeast", will check against yeast motifs
        Peak/BED file conversion summary:
                BED/Header formatted lines: 0
                peakfile formatted lines: 69

        Peak File Statistics:
                Total Peaks: 69
                Redundant Peak IDs: 0
                Peaks lacking information: 0 (need at least 5 columns per peak)
                Peaks with misformatted coordinates: 0 (should be integer)
                Peaks with misformatted strand: 0 (should be either +/- or 0/1)

        Peak file looks good!

        Background files for 200 bp fragments found.

        Extracting sequences from directory: /home/test/homer/.//data/genomes/sacCer2//
        Extracting 47 sequences from chrIV
        Extracting 22 sequences from chrV

        Not removing redundant sequences


        Sequences processed:
                Auto detected maximum sequence length of 201 bp
                69 total

        Frequency Bins: 0.2 0.25 0.3 0.35 0.4 0.45 0.5 0.6 0.7 0.8
        Freq    Bin     Count
        0.35    3       5
        0.4     4       12
        0.45    5       20
        0.5     6       16
        0.6     7       16

        Total sequences set to 50000

        Choosing background that matches in CpG/GC content...
        Bin     # Targets       # Background    Background Weight
        3       5       5541    0.653
        4       12      23190   0.374
        5       20      15393   0.940
        6       16      4709    2.459
        7       16      1098    10.545
        Assembling sequence file...
        Normalizing lower order oligos using homer2

        Reading input files...
        50000 total sequences read
        Autonormalization: 1-mers (4 total)
                A       27.32%  27.91%  0.979
                C       22.68%  22.09%  1.027
                G       22.68%  22.09%  1.027
                T       27.32%  27.91%  0.979
        Autonormalization: 2-mers (16 total)
                AA      9.86%   8.78%   1.123
                CA      6.04%   6.94%   0.872
                GA      5.65%   6.32%   0.894
                TA      5.73%   5.86%   0.977
                AC      5.57%   5.68%   0.981
                CC      5.69%   5.07%   1.123
                GC      5.78%   5.03%   1.149
                TC      5.65%   6.32%   0.894
                AG      5.37%   6.17%   0.871
                CG      5.58%   3.92%   1.422
                GG      5.69%   5.07%   1.123
                TG      6.04%   6.94%   0.872
                AT      6.54%   7.28%   0.899
                CT      5.37%   6.17%   0.871
                GT      5.57%   5.68%   0.981
                TT      9.86%   8.78%   1.123
        Autonormalization: 3-mers (64 total)
        Normalization weights can be found in file: output/part.motif.output/seq.autonorm.tsv
        Converging on autonormalization solution:
        ...............................................................................
        Final normalization:    Autonormalization: 1-mers (4 total)
                A       27.32%  27.93%  0.978
                C       22.68%  22.07%  1.028
                G       22.68%  22.07%  1.028
                T       27.32%  27.93%  0.978
        Autonormalization: 2-mers (16 total)
                AA      9.86%   9.40%   1.049
                CA      6.04%   6.51%   0.928
                GA      5.65%   6.04%   0.935
                TA      5.73%   5.96%   0.961
                AC      5.57%   5.55%   1.003
                CC      5.69%   5.21%   1.093
                GC      5.78%   5.28%   1.094
                TC      5.65%   6.04%   0.935
                AG      5.37%   5.85%   0.918
                CG      5.58%   4.51%   1.237
                GG      5.69%   5.21%   1.093
                TG      6.04%   6.51%   0.928
                AT      6.54%   7.12%   0.919
                CT      5.37%   5.85%   0.918
                GT      5.57%   5.55%   1.003
                TT      9.86%   9.40%   1.049
        Autonormalization: 3-mers (64 total)
        Finished preparing sequence/group files

        ----------------------------------------------------------
        Known motif enrichment

        Reading input files...
        50000 total sequences read
        11 motifs loaded
        Cache length = 11180
        Using binomial scoring
        Checking enrichment of 11 motif(s)
        |0%                                    50%                                  100%|
        =================================================================================
        Preparing HTML output with sequence logos...
                1 of 11 (1e-2) ABF1/SacCer-Promoters/Homer
        ----------------------------------------------------------
        De novo motif finding (HOMER)

        Scanning input files...
        Parsing sequences...
        |0%                                   50%                                  100%|
        ===============================================================================
        Total number of Oligos: 32896
        Autoadjustment for sequence coverage in background: 1.00x

        Oligos: 32896 of 34497 max
        Tree  : 67148 of 172485 max
        Optimizing memory usage...
        Cache length = 11180
        Using binomial scoring

        Global Optimization Phase: Looking for enriched oligos with up to 2 mismatches...

        Screening oligos 32896 (allowing 0 mismatches):
        |0%                                   50%                                  100%|
        ================================================================================
                70.60% skipped, 29.40% checked (9671 of 32896), of those checked:
                70.60% not in target, 0.00% increased p-value, 0.00% high p-value

        Screening oligos 32896 (allowing 1 mismatches):
        |0%                                   50%                                  100%|
        ================================================================================
                70.60% skipped, 29.40% checked (9671 of 32896), of those checked:
                0.00% not in target, 19.54% increased p-value, 15.87% high p-value

        Screening oligos 32896 (allowing 2 mismatches):
        |0%                                   50%                                  100%|
        ================================================================================
                92.48% skipped, 7.52% checked (2473 of 32896), of those checked:
                0.00% not in target, 6.44% increased p-value, 0.00% high p-value
        Reading input files...
        50000 total sequences read
        Cache length = 11180
        Using binomial scoring

        Local Optimization Phase:
        1 of 25 Initial Sequence: CACACCCC... (-42.272)
                Round 1: -58.12 CACACMCC T:65.0(61.28%),B:3424.1(9.38%),P:1e-25
                Round 2: -61.31 MCMCMCAC T:101.0(77.11%),B:6523.8(17.11%),P:1e-26
                Round 3: -61.31 MCMCMCAC T:101.0(77.11%),B:6523.8(17.11%),P:1e-26
                =Final=: -3.44 MCMCMCAC T:28.0(40.58%),B:10254.7(29.50%),P:1e-1
                Performing exhaustive masking of motif...
                Reprioritizing potential motifs...
        2 of 25 Initial Sequence: GGGTTCGA... (-23.933)
                Round 1: -40.97 GGGTTCGA T:30.0(35.47%),B:1170.6(3.31%),P:1e-17
                Round 2: -40.97 GGGTTCGA T:30.0(35.47%),B:1170.6(3.31%),P:1e-17
                =Final=: -39.81 GGGTTCGA T:22.0(31.88%),B:937.5(2.70%),P:1e-17
                Performing exhaustive masking of motif...
                Reprioritizing potential motifs...
        3 of 25 Initial Sequence: TTCCGCGG... (-12.137)
                Round 1: -20.96 TTCCGCGG T:26.0(31.58%),B:2253.3(6.28%),P:1e-9
                Round 2: -25.98 TTCCGCGS T:79.0(68.44%),B:11404.7(27.98%),P:1e-11
                Round 3: -25.98 TTCCGCGS T:79.0(68.44%),B:11404.7(27.98%),P:1e-11
                =Final=: -19.54 TTCCGCGS T:41.0(59.42%),B:8923.0(25.67%),P:1e-8
                Performing exhaustive masking of motif...
                Reprioritizing potential motifs...
        4 of 25 Initial Sequence: AGTGGTTA... (-11.282)
                Round 1: -16.16 AGTGGTTA T:14.0(18.49%),B:850.1(2.42%),P:1e-7
                Round 2: -16.16 AGTGGTTA T:14.0(18.49%),B:850.1(2.42%),P:1e-7
                =Final=: -17.95 AGTGGTTA T:11.0(15.94%),B:565.2(1.63%),P:1e-7
                Performing exhaustive masking of motif...
                Reprioritizing potential motifs...
        5 of 25 Initial Sequence: TTTTTTCT... (-9.635)
                Round 1: -14.11 TTTTTTCT T:31.0(36.40%),B:4803.8(12.91%),P:1e-6
                Round 2: -14.91 ATTTTTAT T:35.0(40.01%),B:5345.9(14.26%),P:1e-6
                Round 3: -14.91 ATTTTTAT T:35.0(40.01%),B:5345.9(14.26%),P:1e-6
                =Final=: -14.60 ATTTTTAT T:26.0(37.68%),B:4698.8(13.52%),P:1e-6
                Performing exhaustive masking of motif...
                Reprioritizing potential motifs...
        6 of 25 Initial Sequence: TAGAAATC... (-9.372)
                Round 1: -9.69 TAGAAATC T:7.0(9.71%),B:342.1(0.98%),P:1e-4
                Round 2: -10.73 TAGAAGTC T:10.0(13.58%),B:779.9(2.22%),P:1e-4
                Round 3: -10.73 TAGAAGTC T:10.0(13.58%),B:779.9(2.22%),P:1e-4
                =Final=: -12.81 TAGAAGTC T:10.0(14.49%),B:764.8(2.20%),P:1e-5
                Performing exhaustive masking of motif...
                Reprioritizing potential motifs...
        7 of 25 Initial Sequence: AACGGCTA... (-8.236)
                Round 1: -9.19 AACGGCTA T:6.0(8.39%),B:232.4(0.67%),P:1e-3
                Round 2: -14.63 CACGGCTA T:12.0(16.07%),B:798.1(2.27%),P:1e-6
                Round 3: -14.63 CACGGCTA T:12.0(16.07%),B:798.1(2.27%),P:1e-6
                =Final=: -14.59 CACGGCTA T:11.0(15.94%),B:792.2(2.28%),P:1e-6
                Performing exhaustive masking of motif...
                Reprioritizing potential motifs...
        Remaining seeds don't look promising (After initial 5 motifs, logp -7.694 > -7.839)

        Finalizing Enrichment Statistics (new in v3.4)
        Reading input files...
        50000 total sequences read
        Cache length = 11180
        Using binomial scoring
        Checking enrichment of 7 motif(s)
        |0%                                    50%                                  100%|
        =================================================================================
        Output in file: output/part.motif.output/homerMotifs.motifs8

        (Motifs in homer2 format)
        Determining similar motifs... 7 reduced to 7 motifs
        Outputing HTML and sequence logos for motif comparison...
        Checking de novo motifs against known motifs...
        Formatting HTML page...
                1 of 7 (1e-17) similar to OPI1/Literature(Harbison)/Yeast(0.940)
                2 of 7 (1e-8) similar to PDR1/MA0352.1/Jaspar(0.797)
                3 of 7 (1e-7) similar to POL004.1_CCAAT-box/Jaspar(0.657)
                4 of 7 (1e-6) similar to POL012.1_TATA-Box/Jaspar(0.749)
                5 of 7 (1e-5) similar to REB1/REB1_YPD/61-REB1(Harbison)/Yeast(0.694)
                6 of 7 (1e-5) similar to POL008.1_DCE_S_I/Jaspar(0.686)
                7 of 7 (1e-1) similar to YPR022C/MA0436.1/Jaspar(0.802)
        Job finished - if results look good, please send beer to ..

        Cleaning up tmp files...

test@bioinfo_docker:~/chip-seq/homework$ cp output/part.motif.output/homerResults.html
home/test/share
cp: cannot create regular file 'home/test/share': No such file or directory
test@bioinfo_docker:~/chip-seq/homework$ cp output/part.motif.output/homerResults.html ~home/test/share
cp: cannot create regular file '~home/test/share': No such file or directory
test@bioinfo_docker:~/chip-seq/homework$ cp output/part.motif.output/homerResults.html /home/test/share
test@bioinfo_docker:~/chip-seq/homework$ cd output/
test@bioinfo_docker:~/chip-seq/homework/output$ ls
part.motif.output  part.peak
test@bioinfo_docker:~/chip-seq/homework/output$ cd part.motif.output/
test@bioinfo_docker:~/chip-seq/homework/output/part.motif.output$ ls
homerMotifs.all.motifs  homerResults.html  knownResults.txt
homerMotifs.motifs8     knownResults       motifFindingParameters.txt
homerResults            knownResults.html  seq.autonorm.tsv
test@bioinfo_docker:~/chip-seq/homework/output/part.motif.output$ cd ..
test@bioinfo_docker:~/chip-seq/homework/output$ cat part.peak head
# HOMER Peaks
# Peak finding parameters:
# tag directory = ip/
#
# total peaks = 69
# peak size = 248
# peaks found using tags on both strands
# minimum distance between peaks = 496
# fragment length = 232
# genome size = 2097191
# Total tags = 1402413.0
# Total tags in peaks = 135829.0
# Approximate IP efficiency = 9.69%
# tags per bp = 0.668669
# expected tags per peak = 165.830
# maximum tags considered per bp = 66.0
# effective number of tags used for normalization = 10000000.0
# Peaks have been centered at maximum tag pile-up
# FDR rate threshold = 0.001000000
# FDR effective poisson threshold = 1.876888e-05
# FDR tag threshold = 222.0
# number of putative peaks = 402
#
# input tag directory = input/
# Fold over input required = 4.00
# Poisson p-value over input required = 1.00e-04
# Putative peaks filtered by input = 323
#
# size of region used for local filtering = 10000
# Fold over local region required = 4.00
# Poisson p-value over local region required = 1.00e-04
# Putative peaks filtered by local signal = 10
#
# Maximum fold under expected unique positions for tags = 2.00
# Putative peaks filtered for being too clonal = 0
#
# cmd = findPeaks ip/ -style factor -o output/part.peak -i input/
#
# Column Headers:
#PeakID chr     start   end     strand  Normalized Tag Count    focus ratio     findPeaks Score        Total Tags      Control Tags (normalized to IP Experiment)      Fold Change vs Control p-value vs Control      Fold Change vs Local    p-value vs Local      Clonal Fold Change
chrIV-1 chrIV   465220  465468  +       111129.9        0.920   15510.000000    15585.0234.1   66.57   0.00e+00        55.11   0.00e+00        0.50
chrIV-2 chrIV   1490100 1490348 +       81687.8 0.857   11468.000000    11456.0 195.1 58.72    0.00e+00        35.06   0.00e+00        0.50
chrV-1  chrV    141138  141386  +       54449.0 0.855   7647.000000     7636.0  182.3 41.88    0.00e+00        21.55   0.00e+00        0.52
chrV-2  chrV    69078   69326   +       48659.0 0.823   6837.000000     6824.0  206.5 33.05    0.00e+00        20.52   0.00e+00        0.50
chrV-3  chrV    85195   85443   +       46277.4 0.861   6493.000000     6490.0  225.6 28.77    0.00e+00        21.56   0.00e+00        0.50
chrIV-3 chrIV   1080509 1080757 +       34405.0 0.832   4830.000000     4825.0  234.1 20.61    0.00e+00        23.11   0.00e+00        0.50
chrIV-4 chrIV   599953  600201  +       26597.0 0.755   3733.000000     3730.0  190.1 19.62    0.00e+00        15.58   0.00e+00        0.50
chrV-4  chrV    321939  322187  +       24821.5 0.754   3484.000000     3481.0  177.4 19.63    0.00e+00        13.66   0.00e+00        0.50
chrIV-5 chrIV   1468786 1469034 +       23595.0 0.794   3317.000000     3309.0  193.7 17.08    0.00e+00        14.36   0.00e+00        0.51
chrIV-6 chrIV   132817  133065  +       19402.3 0.782   2723.000000     2721.0  209.3 13.00    0.00e+00        11.95   0.00e+00        0.52
chrIV-7 chrIV   591669  591917  +       18304.2 0.792   2568.000000     2567.0  200.1 12.83    0.00e+00        12.88   0.00e+00        0.50
chrIV-8 chrIV   721812  722060  +       17840.7 0.739   2514.000000     2502.0  192.3 13.01    0.00e+00        9.61    0.00e+00        0.51
chrIV-9 chrIV   1       230     +       17206.1 0.884   2444.000000     2430.0  60.3  40.30    0.00e+00        632.81  0.00e+00        0.81
chrIV-10        chrIV   1233763 1234011 +       16250.6 0.888   2303.000000     2279.0156.1    14.60   0.00e+00        10.68   0.00e+00        0.52
chrIV-11        chrIV   234340  234588  +       16179.3 0.790   2275.000000     2269.0199.4    11.38   0.00e+00        9.37    0.00e+00        0.51
chrV-5  chrV    225453  225701  +       15066.9 0.901   2123.000000     2113.0  157.5 13.42    0.00e+00        11.73   0.00e+00        0.53
chrIV-12        chrIV   357166  357414  +       15052.6 0.705   2113.000000     2111.0205.7    10.26   0.00e+00        8.19    0.00e+00        0.51
chrIV-13        chrIV   416932  417180  +       13954.5 0.838   1973.000000     1957.0188.0    10.41   0.00e+00        10.83   0.00e+00        0.51
chrIV-15        chrIV   1278678 1278926 +       13697.8 0.852   1925.000000     1921.0225.6    8.51    0.00e+00        12.86   0.00e+00        0.52
chrIV-16        chrIV   1164971 1165219 +       13419.7 0.746   1887.000000     1882.0215.7    8.73    0.00e+00        9.29    0.00e+00        0.52
chrV-6  chrV    491091  491339  +       12457.1 0.773   1749.000000     1747.0  180.9 9.66     0.00e+00        13.69   0.00e+00        0.52
chrV-7  chrV    442162  442410  +       12136.2 0.658   1704.000000     1702.0  247.6 6.87     0.00e+00        10.92   0.00e+00        0.53
chrIV-14        chrIV   1525285 1525496 +       11779.7 0.923   1953.000000     1652.058.2     28.40   0.00e+00        32.51   0.00e+00        1.27
chrV-8  chrV    461852  462100  +       11273.4 0.762   1586.000000     1581.0  227.0 6.96     0.00e+00        8.49    0.00e+00        0.53
chrIV-17        chrIV   722439  722687  +       10774.3 0.676   1512.000000     1511.0172.4    8.76    0.00e+00        5.26    0.00e+00        0.53
chrIV-18        chrIV   550926  551174  +       10688.7 0.686   1503.000000     1499.0237.7    6.31    0.00e+00        5.71    0.00e+00        0.52
chrIV-19        chrIV   411333  411581  +       9783.1  0.812   1380.000000     1372.0203.6    6.74    0.00e+00        7.06    0.00e+00        0.55
chrIV-20        chrIV   894023  894271  +       9597.7  0.767   1357.000000     1346.0267.5    5.03    0.00e+00        7.68    0.00e+00        0.57
chrIV-21        chrIV   1362358 1362606 +       9248.3  0.830   1301.000000     1297.0198.6    6.53    0.00e+00        7.36    0.00e+00        0.55
chrIV-22        chrIV   976499  976747  +       8535.3  0.654   1216.000000     1197.0195.8    6.11    0.00e+00        5.72    0.00e+00        0.54
chrV-10 chrV    153014  153262  +       8178.8  0.629   1147.000000     1147.0  218.5 5.25     0.00e+00        6.20    0.00e+00        0.56
chrIV-23        chrIV   835843  836091  +       8171.6  0.763   1150.000000     1146.0185.9    6.17    0.00e+00        6.43    0.00e+00        0.56
chrV-11 chrV    77613   77861   +       7914.9  0.721   1113.000000     1110.0  214.3 5.18     0.00e+00        5.45    0.00e+00        0.56
chrV-12 chrV    431159  431407  +       7729.5  0.799   1085.000000     1084.0  186.6 5.81     0.00e+00        6.93    0.00e+00        0.57
chrV-13 chrV    174747  174995  +       7529.9  0.805   1056.000000     1056.0  248.3 4.25     1.28e-315       6.25    0.00e+00        0.57
chrV-14 chrV    284625  284873  +       7373.0  0.717   1046.000000     1034.0  189.4 5.46     0.00e+00        4.07    4.50e-294       0.55
chrIV-24        chrIV   368887  369135  +       7244.7  0.708   1025.000000     1016.0200.8    5.06    0.00e+00        5.47    0.00e+00        0.60
chrV-15 chrV    100030  100278  +       7187.6  0.805   1009.000000     1008.0  175.2 5.75     0.00e+00        6.23    0.00e+00        0.64
chrIV-25        chrIV   806247  806495  +       7009.3  0.819   989.000000      983.0 209.3    4.70    0.00e+00        8.06    0.00e+00        0.58
chrIV-27        chrIV   488718  488966  +       6781.2  0.745   952.000000      951.0 156.1    6.09    0.00e+00        5.13    0.00e+00        0.65
chrV-17 chrV    354889  355137  +       6781.2  0.796   973.000000      951.0   150.4 6.32     0.00e+00        7.12    0.00e+00        0.63
chrIV-26        chrIV   974346  974594  +       6766.9  0.849   961.000000      949.0 157.5    6.03    0.00e+00        4.53    6.24e-304       0.58
chrV-16 chrV    135291  135539  +       6667.1  0.869   977.000000      935.0   146.9 6.37     0.00e+00        4.40    3.35e-290       0.70
chrV-18 chrV    311891  312139  +       6667.1  0.739   948.000000      935.0   195.1 4.79     1.72e-317       6.28    0.00e+00        0.61
chrV-19 chrV    242131  242379  +       6659.9  0.649   944.000000      934.0   172.4 5.42     0.00e+00        5.45    0.00e+00        0.57
chrIV-28        chrIV   914926  915174  +       6531.6  0.649   916.000000      916.0 207.9    4.41    5.92e-285       4.64    5.49e-301       0.57
chrV-21 chrV    61809   62057   +       6232.1  0.811   876.000000      874.0   174.5 5.01     3.25e-310       5.98    0.00e+00        0.68
chrV-23 chrV    117822  118070  +       6061.0  0.672   855.000000      850.0   192.3 4.42     1.60e-265       5.64    0.00e+00        0.58
chrV-22 chrV    177069  177317  +       6018.2  0.834   868.000000      844.0   164.6 5.13     1.31e-306       4.85    2.28e-290       0.64
chrV-24 chrV    409297  409545  +       5946.9  0.767   842.000000      834.0   183.0 4.56     4.66e-269       5.05    2.46e-298       0.58
chrIV-29        chrIV   1150723 1150971 +       5925.5  0.879   866.000000      831.0 138.3    6.01    0.00e+00        7.03    0.00e+00        0.76
chrIV-32        chrIV   1017150 1017398 +       5804.3  0.870   823.000000      814.0 159.6    5.10    3.27e-294       5.01    1.84e-289       0.67
chrIV-33        chrIV   1175695 1175943 +       5725.8  0.836   816.000000      803.0 159.6    5.03    1.86e-286       5.64    8.59e-319       0.66
chrIV-36        chrIV   580045  580293  +       5561.8  0.589   792.000000      780.0 193.0    4.04    1.19e-220       4.31    5.44e-237       0.58
chrIV-34        chrIV   434124  434372  +       5355.1  0.813   794.000000      751.0 158.9    4.73    7.61e-252       4.83    9.72e-258       0.69
chrIV-41        chrIV   980844  981092  +       5162.5  0.820   750.000000      724.0 142.6    5.08    7.93e-261       5.31    2.45e-272       0.70
chrIV-44        chrIV   1095341 1095589 +       5048.4  0.865   715.000000      708.0 134.1    5.28    7.62e-265       12.37   0.00e+00        0.68
chrIV-46        chrIV   568825  569073  +       5005.7  0.820   705.000000      702.0 85.8     8.18    0.00e+00        4.54    3.86e-226       0.84
chrIV-47        chrIV   1201659 1201907 +       4977.1  0.808   700.000000      698.0 116.4    6.00    5.79e-293       4.81    1.60e-238       0.67
chrIV-43        chrIV   992784  993032  +       4970.0  0.766   736.000000      697.0 161.0    4.33    3.70e-213       8.77    0.00e+00        0.72
chrIV-49        chrIV   520918  521166  +       4806.0  0.852   684.000000      674.0 152.5    4.42    6.92e-211       6.63    1.64e-307       0.67
chrIV-51        chrIV   1461634 1461882 +       4791.7  0.819   679.000000      672.0 118.5    5.67    1.44e-268       4.38    2.76e-208       0.72
chrIV-52        chrIV   437701  437949  +       4784.6  0.866   678.000000      671.0 155.4    4.32    8.24e-205       4.50    5.07e-214       0.72
chrIV-50        chrIV   619928  620176  +       4727.6  0.838   681.000000      663.0 142.6    4.65    6.64e-219       4.40    2.15e-206       0.70
chrIV-53        chrIV   884320  884568  +       4727.6  0.841   675.000000      663.0 83.7     7.92    0.00e+00        7.91    0.00e+00        0.89
chrIV-57        chrIV   83428   83676   +       4535.0  0.803   638.000000      636.0 158.2    4.02    3.45e-179       4.94    3.78e-223       0.67
chrIV-58        chrIV   946285  946533  +       4456.6  0.792   638.000000      625.0 139.1    4.49    2.63e-199       5.04    5.13e-224       0.76
chrIV-59        chrIV   1305493 1305741 +       4392.4  0.801   630.000000      616.0 127.7    4.82    2.57e-211       4.07    6.79e-176       0.74
chrIV-63        chrIV   802596  802844  +       4057.3  0.833   591.000000      569.0 139.8    4.07    6.77e-163       4.49    2.24e-181       0.68
cat: head: No such file or directory
test@bioinfo_docker:~/chip-seq/homework/output$ cat part.peak | head
# HOMER Peaks
# Peak finding parameters:
# tag directory = ip/
#
# total peaks = 69
# peak size = 248
# peaks found using tags on both strands
# minimum distance between peaks = 496
# fragment length = 232
# genome size = 2097191
test@bioinfo_docker:~/chip-seq/homework/output$ cp part.peak /home/test/share
```


![image](https://github.com/user-attachments/assets/e2fcb771-8de8-4da7-9816-a83ad26ba0dc)




