## <p align="center">lab_419--工具集</p>

## <p align="center">目录</p>
[sample](#sample)：新增例子
#### 一、数据获取

- 1、[ncbi](https://www.ncbi.nlm.nih.gov/)
    查找SRR号通过sratoolkit下载
- 2、[ncbi_jp](https://trace.ddbj.nig.ac.jp/DRASearch/)
    获取链接通过wget下载
- 3、 [hmp](https://www.hmpdacc.org/ihmp/)
- 4、 [Aspera](ascp.md):快速下载SRR等文件(在服务器8888上，已添加在环境变量中）
      

#### 二、[数据处理](#数据处理)
- 1、[kmc](#kmc)：计算kmer频度
- 2、[metaphlan](#metaphlan):测序数据转丰度数据
- 3、[sratoolkit](#sratoolkit)：下载srr文件，并转换为fasta格式
- 4、[cap3](#cap3)：拼装短序列
- 5、fastq转fasta
```
awk '{if(NR%4 == 1){print ">" substr($0, 2)}}{if(NR%4 == 2){print}}' fastq > fasta
```

#### 三、[距离计算](距离计算)
- 1、[cafe](#cafe)：多种距离计算方式
- 2、[Afnn](#Afann)：alignment-free计算方法
- 3、[mash](#mash):依据kmer缺失计算
- 4、[skmer](#skmer):mash优化后的工具

#### 四、[数据分析](数据分析)
- 1、[mega](mega)：距离矩阵转进化树图
- 2、树距离计算
  - 2.1、[philp](#philp):Symmetric difference
  - 2.2、[Mothur](#Mothur)::Parsimony
  - 2.3、[TreeCmp](#TreeCmp):the triples distance




## <p align="center">工具介绍：</p>
#### <span id="sample">sample:</span>
- 简易使用：
``` 
sample：新增
```
- 支持文档：
https://sample.com

### <span id="数据处理"><p align="center">二、数据处理:</p></span>
#### <span id="kmc">kmc:</span>
- 简易使用：
```
kmc -k8 -ci2 -cs65536000 -t10 -fm -b -v input output1 ./
kmc_dump2 ouput1  ouput 
#output1由一产生，kmc_dump2产生二进制文件，kmc_dump产生十进制
```
- 支持文档：
http://sun.aei.polsl.pl/REFRESH/index.php?page=projects&project=kmc&subpage=about

#### <span id="metaphlan">metaphlan:</span>
- 简易使用：
``` 
 metaphlan2.py SRS014476-Supragingival_plaque.fasta.gz  --input_type fasta > SRS014476-Supragingival_plaque_profile.txt
单线程
zcat  CSM9X23N_R1.fastq.gz CSM9X23N_R2.fastq.gz | metaphlan --input_type fastq 
//一般使用双端fastq作管道输入

--nproc 4 。4线程

 merge_metaphlan_tables.py *_profile.txt > merged_abundance_table.txt
将所有的结果合起来

 metaphlan_hclust_heatmap.py -c bbcry --top 25 --minv 0.1 -s log --in merged_metaphlan2.txt --out abundance_heatmap.pdf
可视化
```
- 支持文档：
https://github.com/biobakery/biobakery/wiki/metaphlan2

#### <span id="sratoolkit">sratoolkit:</span>
- 简易使用：
``` 
/home/yingwang/data1/wangying/wangkun/sratoolkit.2.9.0-ubuntu64/bin/prefetch -X 41943040 SRR8307275    ----下载

fastq-dump  ./SRR***.sra -O ./   ---转fastq

fastq-dump  --fasta  ./SRR306998.sra  -O ./   ----转---fasta
```
- 支持文档：
https://www.cnblogs.com/OA-maque/p/4799074.html

#### <span id="cap3">cap3:</span>
- 简易使用：
``` 
/home/yingwang/tools/cap3 input.fa -i 30 -j 31 -o 18 -s 300

```




### <span id="距离计算"><p align="center">三、距离计算:</p></span>
#### <span id="cafe">cafe:</span>
- 简易使用：
```
 ./cafe_linux -M 0 -O 27ecoli -S 27ecoli -T plain -I  /home/yingwang/jiaxing/27ecoli/E.coli-APECO1.fna,/home/yingwang/jiaxing/27ecoli/E.coli-C-ATCC-8739.fna  -K 14 -D Ma
```
- 支持文档：
https://github.com/younglululu/CAFE

#### <span id="Afann">Afann:</span>
- 简易使用：
```
python afann.py -r -a d2star,d2shepp -k 5 -m 0 -f test_file.txt -t 8 -d test_count/ -o test_result/ --adjust
```
- 支持文档：
https://github.com/GeniusTang/Afann

#### <span id="mash">mash:</span>
- 简易使用：
```
./mash sketch /home/yingwang/jiaxing/MammalianGut_Fna/*C.fna  /home/yingwang/jiaxing/MammalianGut_Fna/*H.fna -k 14 -o mammaliangutmash14
./mash dist mammaliangutmash14.msh mammaliangutmash14.msh >mammaliangutmash14dist
```
- 支持文档：
https://github.com/marbl/Mash/releases

#### <span id="skmer">skmer:</span>
- 简易使用：
```
构建参考树
skmer reference ref_dir -p 4 -o [distance_dir] -l [library_dir]
计算数据库中两两距离
skmer distance library -t(tree) -o jc-dist-mat
计算输入序列与数据库的距离
skmer query qry.fastq library -o output_prefix
```
- 支持文档：
https://github.com/shahab-sarmashghi/Skmer




### <span id="数据分析"><p align="center">四、数据分析</p></span>
#### <span id="mega">mega:</span>
- 简易使用：
输入是R处理好的nwk文件，可以多棵树同时操作，放大缩小加颜色，
- 支持文档：
https://www.megasoftware.net/


#### <span id="philp">philp:</span>
- 简易使用：
``` 
计算 Symmetric Difference
cmd进入exe目录，开启treedist.exe程序：
H:\my_task\CRAFT\实验结果数据\利用距离矩阵建树\树之间的距离计算\phylip-3.695\27four_method.nwk
修改D(D回车可以修改,为所要的距离)
修改2为P（所有的树互相计算距离）
F全矩阵输出
```
- 支持文档：
http://evolution.gs.washington.edu/phylip.html

#### <span id="Mothur">Mothur:</span>
- 简易使用：
``` 
parsimony(tree=28.nj,group=28mammalian_gut.groups)
parsimony(tree=27.nj, group=27ecoli.groups)
```
- 支持文档：
http://www.mothur.org/wiki

#### <span id="TreeCmp">TreeCmp:</span>
- 简易使用：
```
java -jar H:\my_task\TreeCmp\TreeCmp\bin\TreeCmp.jar -m -d qt -i 28four_method.nwk -o 28four_method.nwk.out
```
- 支持文档：
- https://github.com/TreeCmp/TreeCmp
