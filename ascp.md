## NCBI 下载数据
- 先在GEO里面找打你的目的数据，及其SRR号（举例）：SRR653396
- 在FTP位置ftp://ftp.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByRun/sra/SRR/找到该文件：ftp://ftp.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByRun/sra/SRR/SRR653/SRR653396/SRR653396.sra
替换以下代码
~/biosoft/anaconda/etc/asperaweb_id_dsa.openssh为你自己安装后的openssh位置
/sra/sra-instant/reads/ByRun/sra/SRR/SRR653/SRR653396/SRR653396.sra为文件ftp地址的路径，去掉开头的地址然后使用公用账号anonftp进行下载，最后成型的
- 下载代码为：
```
ascp -v -k 1 -T -l 200m -i ~/biosoft/anaconda/etc/asperaweb_id_dsa.openssh anonftp@ftp-private.ncbi.nlm.nih.gov:/sra/sra-instant/reads/ByRun/sra/SRR/SRR653/SRR653396/SRR653396.sra ./
```
