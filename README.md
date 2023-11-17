# somatico-vep
Pipeline Somático - Anotação ensembl-vep


### aria2 - fast download
```bash
brew install aria2
```

### download VEP cache indexed - homo_sapiens_merged_110_GRCh37.zip
```
aria2c -x 8 https://storage.googleapis.com/puga-reference/homo_sapiens_merged_110_GRCh37.zip
```

### google cloud - bucket puga-reference
```
aria2c -x 8 https://storage.googleapis.com/puga-reference/homo_sapiens_merged_110_GRCh37.zip
```

### unzip - decompactar 
```
unzip homo_sapiens_merged_110_GRCh37.zip
```


