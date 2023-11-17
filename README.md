# somatico-vep
Pipeline Somático - Anotação ensembl-vep

### gitpod - configuration

<img width="814" alt="Screenshot 2023-11-17 at 19 20 13" src="https://github.com/renatopuga/somatico-vep/assets/8321336/4259c5ad-1cb4-4501-905c-7a306d1f8060">

### aria2 - fast download

```bash
brew install aria2
```

### download VEP cache indexed - homo_sapiens_merged_110_GRCh37.zip

```bash
aria2c -x 8 https://storage.googleapis.com/puga-reference/homo_sapiens_merged_110_GRCh37.zip
```

### unzip - decompactar 

```bash
unzip homo_sapiens_merged_110_GRCh37.zip
```


