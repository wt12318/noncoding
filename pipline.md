

## 下载数据

### 突变数据

ICGC prostate cancer, WGS, SSM(简单体细胞突变)，有858donors, 227155个突变位点

存放位置：

学校服务器（wangshx@10.15.22.110）```/public/home/wangshx/wt/noncoding/prostatecancer```

### 注释文件

-  Long non-coding RNA gene annotation ```https://www.gencodegenes.org/human/release_32lift37.html``` 选择的是gtf文件，但下载后发现文件开头的注释里的格式仍然是gff3格式
- TSS(转录起始位点) ```http://fantom.gsc.riken.jp/5/datafiles/phase1.3/extra/TSS_classifier/``` 下载```TSS_human.bed.gz```
- TFBS(转录因子结合位点) ```http://hgdownload.soe.ucsc.edu/goldenPath/hg19/encodeDCC/wgEncodeRegTfbsClustered/``` 下载```wgEncodeRegTfbsClusteredWithCellsV3.bed.gz```
- DHS(DNA酶超敏感位点，开放染色质) ```http://big.databio.org/papers/RED/supplement/``` 下载```TableS03-dhs-to-cluster.txt.tar.gz``` 内有两个txt文件：```TableS03-dhs-to-cluster```和```TableS04-cluster-to-openCellTypes_v3```,根据第二个文件筛选出prostate的cluster，然后与第一个文件匹配找出对应的DHS位点
- enhancer ``` http://fantom.gsc.riken.jp/5/datafiles/latest/extra/Enhancers/``` 下载 ```human_permissive_enhancers_phase_1_and_2.bed.gz```
- promoter ```http://fantom.gsc.riken.jp/5/datafiles/latest/extra/CAGE_peaks/``` 下载```hg19.cage_peak_phase1and2combined_coord.bed.gz```

## 文件格式转化

```shell
##gtf(实际上是gff3)转bed
gffread longcodingRNA.gff3 -T -o noncodingRNA.gtf
 
cat noncodingRNA.gtf | cut -f1,4,5,9 | cut -f1 -d";" | awk '{print $1, $2, $3, $5}' | sed -e 's/ /\t/g' | sed -e 's/\"//g' | sed -e 's/transcript\://g' > noncodingRNA.bed 

##txt转bed
##直接在R里面操作
A <- read.table("DHS.bed.txt")
write.table(A,file="DHS.bed",sep="\t",row.names = FALSE,col.names = FALSE,quote = FALSE)

```

## 模型构建

