h2. Comparison on compression

compression off
<pre>
Writing intelligently...^Croot@romano:/cihmz/collections/bonnie# bonnie++ -u rob
Using uid:1102, gid:100.
Writing a byte at a time...done
Writing intelligently...done
Rewriting...done
Reading a byte at a time...done
Reading intelligently...done
start 'em...done...done...done...done...done...
Create files in sequential order...done.
Stat files in sequential order...done.
Delete files in sequential order...done.
Create files in random order...done.
Stat files in random order...done.
Delete files in random order...done.
Version  1.96       ------Sequential Output------ --Sequential Input- --Random-
Concurrency   1     -Per Chr- --Block-- -Rewrite- -Per Chr- --Block-- --Seeks--
Machine        Size K/sec %CP K/sec %CP K/sec %CP K/sec %CP K/sec %CP  /sec %CP
romano         252G   136  99 590366  98 212487  52   294  98 386851  40 161.6  16
Latency             89204us   13696us    1411ms   57731us     468ms     235ms
Version  1.96       ------Sequential Create------ --------Random Create--------
romano              -Create-- --Read--- -Delete-- -Create-- --Read--- -Delete--
              files  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP
                 16 14246  79 +++++ +++ 16405  82 13710  82 +++++ +++ 17775  99
Latency             39236us     505us     535us   79344us      29us     120us
1.96,1.96,romano,1,1389794445,252G,,136,99,590366,98,212487,52,294,98,386851,40,161.6,16,16,,,,,14246,79,+++++,+++,16405,82,13710,82,+++++,+++,17775,99,89204us,13696us,1411ms,57731us,468ms,235ms,39236us,505us,535us,79344us,29us,120us
root@romano:/cihmz/collections/bonnie#
</pre>

with compression
<pre>
root@romano:/cihmz/collections/bonnie# bonnie++ -u rob
Using uid:1102, gid:100.
Writing a byte at a time...done
Writing intelligently...done
Rewriting...done
Reading a byte at a time...done
Reading intelligently...done
start 'em...done...done...done...done...done...
Create files in sequential order...done.
Stat files in sequential order...done.
Delete files in sequential order...done.
Create files in random order...done.
Stat files in random order...done.
Delete files in random order...done.
Version  1.96       ------Sequential Output------ --Sequential Input- --Random-
Concurrency   1     -Per Chr- --Block-- -Rewrite- -Per Chr- --Block-- --Seeks--
Machine        Size K/sec %CP K/sec %CP K/sec %CP K/sec %CP K/sec %CP  /sec %CP
romano         252G   136  99 727595  96 369089  87   299  99 812572  82 195.1  19
Latency               108ms   47593us     120ms   59908us   80766us     206ms
Version  1.96       ------Sequential Create------ --------Random Create--------
romano              -Create-- --Read--- -Delete-- -Create-- --Read--- -Delete--
              files  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP
                 16 14634  86 +++++ +++ 15918  88 12799  99 +++++ +++ 13486  85
Latency             33507us     459us     526us   81003us      46us   31141us
1.96,1.96,romano,1,1389789469,252G,,136,99,727595,96,369089,87,299,99,812572,82,195.1,19,16,,,,,14634,86,+++++,+++,15918,88,12799,99,+++++,+++,13486,85,108ms,47593us,120ms,59908us,80766us,206ms,33507us,459us,526us,81003us,46us,31141us
</pre>

h2. parallelism and bonnie

<pre>

root@romano:/cihmz/collections/bonnie# bonnie++ -u rob -p3

root@romano:/cihmz/collections/bonnie# bonnie++ -u rob -ys > out1 &
root@romano:/cihmz/collections/bonnie# bonnie++ -u rob -ys > out2 &
root@romano:/cihmz/collections/bonnie# bonnie++ -u rob -ys > out3 &

root@romano:/cihmz/collections/bonnie# cat out*
Version  1.96       ------Sequential Output------ --Sequential Input- --Random-
Concurrency   1     -Per Chr- --Block-- -Rewrite- -Per Chr- --Block-- --Seeks--
Machine        Size K/sec %CP K/sec %CP K/sec %CP K/sec %CP K/sec %CP  /sec %CP
romano         252G    53  92 381085  92 209780  85   115  99 517014  77  73.1   7
Latency               157ms   95967us     197ms   93065us     153ms     288ms
Version  1.96       ------Sequential Create------ --------Random Create--------
romano              -Create-- --Read--- -Delete-- -Create-- --Read--- -Delete--
              files  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP
                 16  5715  76 +++++ +++  2735  82  4029  53 +++++ +++  2442  95
Latency               192ms    1101us    6678us     269ms      20us    8441us
1.96,1.96,romano,1,1389790439,252G,,53,92,381085,92,209780,85,115,99,517014,77,73.1,7,16,,,,,5715,76,+++++,+++,2735,82,4029,53,+++++,+++,2442,95,157ms,95967us,197ms,93065us,153ms,288ms,192ms,1101us,6678us,269ms,20us,8441us
Version  1.96       ------Sequential Output------ --Sequential Input- --Random-
Concurrency   1     -Per Chr- --Block-- -Rewrite- -Per Chr- --Block-- --Seeks--
Machine        Size K/sec %CP K/sec %CP K/sec %CP K/sec %CP K/sec %CP  /sec %CP
romano         252G    55  96 384934  92 210388  85   118  99 518116  77  75.6   7
Latency               157ms   57460us     184ms     111ms     186ms     278ms
Version  1.96       ------Sequential Create------ --------Random Create--------
romano              -Create-- --Read--- -Delete-- -Create-- --Read--- -Delete--
              files  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP
                 16  5314  58 +++++ +++  2735  79  4030  58 +++++ +++  2289  93
Latency               191ms     417us    8805us     269ms      31us     138ms
1.96,1.96,romano,1,1389790435,252G,,55,96,384934,92,210388,85,118,99,518116,77,75.6,7,16,,,,,5314,58,+++++,+++,2735,79,4030,58,+++++,+++,2289,93,157ms,57460us,184ms,111ms,186ms,278ms,191ms,417us,8805us,269ms,31us,138ms
Version  1.96       ------Sequential Output------ --Sequential Input- --Random-
Concurrency   1     -Per Chr- --Block-- -Rewrite- -Per Chr- --Block-- --Seeks--
Machine        Size K/sec %CP K/sec %CP K/sec %CP K/sec %CP K/sec %CP  /sec %CP
romano         252G    59  99 391810  92 214531  85   118  99 524927  78  70.5   7
Latency               156ms   71561us     466ms     111ms     162ms     310ms
Version  1.96       ------Sequential Create------ --------Random Create--------
romano              -Create-- --Read--- -Delete-- -Create-- --Read--- -Delete--
              files  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP
                 16  6688  64 +++++ +++  2735  82  4331  58 +++++ +++  2183  92
Latency               191ms     745us    8585us     269ms      21us     138ms
1.96,1.96,romano,1,1389790446,252G,,59,99,391810,92,214531,85,118,99,524927,78,70.5,7,16,,,,,6688,64,+++++,+++,2735,82,4331,58,+++++,+++,2183,92,156ms,71561us,466ms,111ms,162ms,310ms,191ms,745us,8585us,269ms,21us,138ms
</pre>

6 procs
<pre>
Version  1.96       ------Sequential Output------ --Sequential Input- --Random-
Concurrency   1     -Per Chr- --Block-- -Rewrite- -Per Chr- --Block-- --Seeks--
Machine        Size K/sec %CP K/sec %CP K/sec %CP K/sec %CP K/sec %CP  /sec %CP
romano         252G    32  96 229057  83 128291  80    76  96 379120  75  42.6   4
Latency               294ms    1679ms    3348ms     189ms     295ms     534ms
Version  1.96       ------Sequential Create------ --------Random Create--------
romano              -Create-- --Read--- -Delete-- -Create-- --Read--- -Delete--
              files  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP
                 16  2288  38 +++++ +++  1848  87  1731  36 +++++ +++  1890  79
Latency               400ms     708us     130ms     608ms     158us     152ms
1.96,1.96,romano,1,1389862414,252G,,32,96,229057,83,128291,80,76,96,379120,75,42.6,4,16,,,,,2288,38,+++++,+++,1848,87,1731,36,+++++,+++,1890,79,294ms,1679ms,3348ms,189ms,295ms,534ms,400ms,708us,130ms,608ms,158us,152ms
Version  1.96       ------Sequential Output------ --Sequential Input- --Random-
Concurrency   1     -Per Chr- --Block-- -Rewrite- -Per Chr- --Block-- --Seeks--
Machine        Size K/sec %CP K/sec %CP K/sec %CP K/sec %CP K/sec %CP  /sec %CP
romano         252G    31  96 224993  83 125922  81    75  96 375855  76  46.1   4
Latency               299ms    1682ms    3463ms     172ms     820ms     580ms
Version  1.96       ------Sequential Create------ --------Random Create--------
romano              -Create-- --Read--- -Delete-- -Create-- --Read--- -Delete--
              files  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP
                 16  2196  41 +++++ +++  1772  84  1711  32 +++++ +++  1934  78
Latency               400ms     575us     130ms     608ms    3121us   24966us
1.96,1.96,romano,1,1389862413,252G,,31,96,224993,83,125922,81,75,96,375855,76,46.1,4,16,,,,,2196,41,+++++,+++,1772,84,1711,32,+++++,+++,1934,78,299ms,1682ms,3463ms,172ms,820ms,580ms,400ms,575us,130ms,608ms,3121us,24966us
Version  1.96       ------Sequential Output------ --Sequential Input- --Random-
Concurrency   1     -Per Chr- --Block-- -Rewrite- -Per Chr- --Block-- --Seeks--
Machine        Size K/sec %CP K/sec %CP K/sec %CP K/sec %CP K/sec %CP  /sec %CP
romano         252G    31  96 225299  83 126513  80    76  96 374056  75  39.7   3
Latency               301ms    1681ms    3445ms     165ms     847ms     744ms
Version  1.96       ------Sequential Create------ --------Random Create--------
romano              -Create-- --Read--- -Delete-- -Create-- --Read--- -Delete--
              files  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP
                 16  2237  38 +++++ +++  1885  87  1835  35 +++++ +++  1758  72
Latency               399ms     448us     129ms     608ms     157us     153ms
1.96,1.96,romano,1,1389862412,252G,,31,96,225299,83,126513,80,76,96,374056,75,39.7,3,16,,,,,2237,38,+++++,+++,1885,87,1835,35,+++++,+++,1758,72,301ms,1681ms,3445ms,165ms,847ms,744ms,399ms,448us,129ms,608ms,157us,153ms
Version  1.96       ------Sequential Output------ --Sequential Input- --Random-
Concurrency   1     -Per Chr- --Block-- -Rewrite- -Per Chr- --Block-- --Seeks--
Machine        Size K/sec %CP K/sec %CP K/sec %CP K/sec %CP K/sec %CP  /sec %CP
romano         252G    31  96 225422  83 126163  80    75  96 374480  75  41.5   3
Latency               301ms    1681ms    3394ms     153ms     324ms     678ms
Version  1.96       ------Sequential Create------ --------Random Create--------
romano              -Create-- --Read--- -Delete-- -Create-- --Read--- -Delete--
              files  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP
                 16  2591  45 +++++ +++  1726  86  1684  29 +++++ +++  1732  74
Latency               400ms     685us     133ms     608ms     136us     152ms
1.96,1.96,romano,1,1389862419,252G,,31,96,225422,83,126163,80,75,96,374480,75,41.5,3,16,,,,,2591,45,+++++,+++,1726,86,1684,29,+++++,+++,1732,74,301ms,1681ms,3394ms,153ms,324ms,678ms,400ms,685us,133ms,608ms,136us,152ms
Version  1.96       ------Sequential Output------ --Sequential Input- --Random-
Concurrency   1     -Per Chr- --Block-- -Rewrite- -Per Chr- --Block-- --Seeks--
Machine        Size K/sec %CP K/sec %CP K/sec %CP K/sec %CP K/sec %CP  /sec %CP
romano         252G    31  96 225682  83 126499  80    76  96 377311  75  39.2   3
Latency               300ms    1682ms    3343ms     152ms     381ms     680ms
Version  1.96       ------Sequential Create------ --------Random Create--------
romano              -Create-- --Read--- -Delete-- -Create-- --Read--- -Delete--
              files  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP
                 16  2444  46 +++++ +++  1932  88  1754  30 +++++ +++  1846  77
Latency               399ms     477us   13020us     608ms     123us     152ms
1.96,1.96,romano,1,1389862418,252G,,31,96,225682,83,126499,80,76,96,377311,75,39.2,3,16,,,,,2444,46,+++++,+++,1932,88,1754,30,+++++,+++,1846,77,300ms,1682ms,3343ms,152ms,381ms,680ms,399ms,477us,13020us,608ms,123us,152ms
Version  1.96       ------Sequential Output------ --Sequential Input- --Random-
Concurrency   1     -Per Chr- --Block-- -Rewrite- -Per Chr- --Block-- --Seeks--
Machine        Size K/sec %CP K/sec %CP K/sec %CP K/sec %CP K/sec %CP  /sec %CP
romano         252G    31  96 226213  83 126380  80    76  96 374211  75  45.5   4
Latency               296ms    1680ms    3423ms     191ms     380ms     625ms
Version  1.96       ------Sequential Create------ --------Random Create--------
romano              -Create-- --Read--- -Delete-- -Create-- --Read--- -Delete--
              files  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP
                 16  2517  39 +++++ +++  1798  87  1803  46 +++++ +++  1797  77
Latency               400ms     464us     134ms     608ms     152us     152ms
1.96,1.96,romano,1,1389862417,252G,,31,96,226213,83,126380,80,76,96,374211,75,45.5,4,16,,,,,2517,39,+++++,+++,1798,87,1803,46,+++++,+++,1797,77,296ms,1680ms,3423ms,191ms,380ms,625ms,400ms,464us,134ms,608ms,152us,152ms
</pre>

with ssd cache
<pre>
root@romano:/cihmz/collections/bonnie# cat out-cache-*
Version  1.96       ------Sequential Output------ --Sequential Input- --Random-
Concurrency   1     -Per Chr- --Block-- -Rewrite- -Per Chr- --Block-- --Seeks--
Machine        Size K/sec %CP K/sec %CP K/sec %CP K/sec %CP K/sec %CP  /sec %CP
romano         252G    31  96 225273  84 129832  84    68  95 409520  88  89.3  22
Latency               302ms    3841ms    5382ms     224ms     803ms     581ms
Version  1.96       ------Sequential Create------ --------Random Create--------
romano              -Create-- --Read--- -Delete-- -Create-- --Read--- -Delete--
              files  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP
                 16  2835  45 +++++ +++  1244  86  1221  23 +++++ +++  2366  91
Latency               450ms     695us     136ms     573ms      96us   14025us
1.96,1.96,romano,1,1389868560,252G,,31,96,225273,84,129832,84,68,95,409520,88,89.3,22,16,,,,,2835,45,+++++,+++,1244,86,1221,23,+++++,+++,2366,91,302ms,3841ms,5382ms,224ms,803ms,581ms,450ms,695us,136ms,573ms,96us,14025us
Version  1.96       ------Sequential Output------ --Sequential Input- --Random-
Concurrency   1     -Per Chr- --Block-- -Rewrite- -Per Chr- --Block-- --Seeks--
Machine        Size K/sec %CP K/sec %CP K/sec %CP K/sec %CP K/sec %CP  /sec %CP
romano         252G    31  97 221336  84 127736  84    68  96 402896  88  75.9  20
Latency               305ms    3836ms    4974ms     207ms     797ms     552ms
Version  1.96       ------Sequential Create------ --------Random Create--------
romano              -Create-- --Read--- -Delete-- -Create-- --Read--- -Delete--
              files  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP
                 16  3421  49 +++++ +++  1449  87  1291  29 +++++ +++  2401  91
Latency               308ms     695us   33470us     765ms      95us   16011us
1.96,1.96,romano,1,1389868561,252G,,31,97,221336,84,127736,84,68,96,402896,88,75.9,20,16,,,,,3421,49,+++++,+++,1449,87,1291,29,+++++,+++,2401,91,305ms,3836ms,4974ms,207ms,797ms,552ms,308ms,695us,33470us,765ms,95us,16011us
Version  1.96       ------Sequential Output------ --Sequential Input- --Random-
Concurrency   1     -Per Chr- --Block-- -Rewrite- -Per Chr- --Block-- --Seeks--
Machine        Size K/sec %CP K/sec %CP K/sec %CP K/sec %CP K/sec %CP  /sec %CP
romano         252G    31  97 223700  84 128717  84    68  96 406631  88  89.9  22
Latency               295ms    3841ms    5362ms     213ms     421ms     779ms
Version  1.96       ------Sequential Create------ --------Random Create--------
romano              -Create-- --Read--- -Delete-- -Create-- --Read--- -Delete--
              files  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP
                 16  2792  42 +++++ +++  1281  87  1295  32 +++++ +++  2360  91
Latency               450ms     708us   31746us     759ms      73us   16961us
1.96,1.96,romano,1,1389868574,252G,,31,97,223700,84,128717,84,68,96,406631,88,89.9,22,16,,,,,2792,42,+++++,+++,1281,87,1295,32,+++++,+++,2360,91,295ms,3841ms,5362ms,213ms,421ms,779ms,450ms,708us,31746us,759ms,73us,16961us
Version  1.96       ------Sequential Output------ --Sequential Input- --Random-
Concurrency   1     -Per Chr- --Block-- -Rewrite- -Per Chr- --Block-- --Seeks--
Machine        Size K/sec %CP K/sec %CP K/sec %CP K/sec %CP K/sec %CP  /sec %CP
romano         252G    31  97 221617  84 127588  84    68  95 402634  88  88.1  23
Latency               302ms    3841ms    4684ms     230ms    1127ms     653ms
Version  1.96       ------Sequential Create------ --------Random Create--------
romano              -Create-- --Read--- -Delete-- -Create-- --Read--- -Delete--
              files  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP
                 16  2623  42 +++++ +++  1252  86  1224  48 +++++ +++  2416  91
Latency               450ms     681us   33282us     573ms   12041us   14137us
1.96,1.96,romano,1,1389868575,252G,,31,97,221617,84,127588,84,68,95,402634,88,88.1,23,16,,,,,2623,42,+++++,+++,1252,86,1224,48,+++++,+++,2416,91,302ms,3841ms,4684ms,230ms,1127ms,653ms,450ms,681us,33282us,573ms,12041us,14137us
Version  1.96       ------Sequential Output------ --Sequential Input- --Random-
Concurrency   1     -Per Chr- --Block-- -Rewrite- -Per Chr- --Block-- --Seeks--
Machine        Size K/sec %CP K/sec %CP K/sec %CP K/sec %CP K/sec %CP  /sec %CP
romano         252G    31  96 222264  84 128102  84    69  96 403958  88  75.8  20
Latency               299ms    3841ms    4677ms     207ms     497ms     583ms
Version  1.96       ------Sequential Create------ --------Random Create--------
romano              -Create-- --Read--- -Delete-- -Create-- --Read--- -Delete--
              files  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP
                 16  2584  48 +++++ +++  1329  87  1217  28 +++++ +++  2390  90
Latency               450ms     691us   33335us     758ms   12392us   15064us
1.96,1.96,romano,1,1389868572,252G,,31,96,222264,84,128102,84,69,96,403958,88,75.8,20,16,,,,,2584,48,+++++,+++,1329,87,1217,28,+++++,+++,2390,90,299ms,3841ms,4677ms,207ms,497ms,583ms,450ms,691us,33335us,758ms,12392us,15064us
Version  1.96       ------Sequential Output------ --Sequential Input- --Random-
Concurrency   1     -Per Chr- --Block-- -Rewrite- -Per Chr- --Block-- --Seeks--
Machine        Size K/sec %CP K/sec %CP K/sec %CP K/sec %CP K/sec %CP  /sec %CP
romano         252G    31  97 222040  84 127707  84    68  95 403042  88  75.8  20
Latency               305ms    3842ms    5383ms     199ms     580ms     643ms
Version  1.96       ------Sequential Create------ --------Random Create--------
romano              -Create-- --Read--- -Delete-- -Create-- --Read--- -Delete--
              files  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP  /sec %CP
                 16  2605  46 +++++ +++  1255  86  1284  20 +++++ +++  2515  92
Latency               450ms     699us     105ms     573ms     132us    9342us
1.96,1.96,romano,1,1389868573,252G,,31,97,222040,84,127707,84,68,95,403042,88,75.8,20,16,,,,,2605,46,+++++,+++,1255,86,1284,20,+++++,+++,2515,92,305ms,3842ms,5383ms,199ms,580ms,643ms,450ms,699us,105ms,573ms,132us,9342us
</pre>
