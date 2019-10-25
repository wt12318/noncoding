```shell
 ##gtf(实际上是gff3)转bed
gffread longcodingRNA.gff3 -T -o noncodingRNA.gtf
 
cat noncodingRNA.gtf | cut -f1,4,5,9 | cut -f1 -d";" | awk '{print $1, $2, $3, $5}' | sed -e 's/ /\t/g' | sed -e 's/\"//g' | sed -e 's/transcript\://g' > noncodingRNA.bed 
```

