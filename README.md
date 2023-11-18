# somatico-vep
Pipeline Somático - Anotação ensembl-vep

### gitpod - configuration

<img width="514" alt="Screenshot 2023-11-17 at 19 20 13" src="https://github.com/renatopuga/somatico-vep/assets/8321336/4259c5ad-1cb4-4501-905c-7a306d1f8060">

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

### hg19.fa

- download UCSC hg19.fa.gz

```bash
aria2c -x 5 https://hgdownload.soe.ucsc.edu/goldenPath/hg19/bigZips/hg19.fa.gz
```

- descompactar

```bash
gunzip hg19.fa.gz
```

- mover para o diretório homo_sapiens_merged

```bash
mv hg19.fa homo_sapiens_merged
```

```bash
chmod 777 homo_sapiens_merged/
```

### VEP


### docker pull vep
```bash
docker pull ensemblorg/ensembl-vep
```

### WP312 - LMA sample

- WP312.filtered.vcf.gz
- WP312.filtered.vcf.gz.tbi

https://drive.google.com/drive/u/0/folders/1m2qmd0ca2Nwb7qcK58ER0zC8-1_9uAiE


### criando diretório de output

```bash
mkdir -p vep_output
```

### permission 

```bash
chmod 777 vep_output
```

### rodar vep

```bash
docker run -it --rm  -v $(pwd):/data ensemblorg/ensembl-vep vep \
-i /data/WP312.filtered.vcf.gz \
-o /data/vep_output/WP312.filtered.vep.tsv \
--assembly GRCh37  \
--merged -pick \
--pick_allele \
--force_overwrite \
--tab --symbol --distance 0 \
--fields "Location,SYMBOL,Consequence,Feature,Amino_acids,CLIN_SIG" \
--individual all \
--dir_cache /data/ \
--cache --offline \
--check_existing \
--fork 10 \
--fasta /data/homo_sapiens_merged/hg19.fa
```

### VEP - Mais opções

Todas as colunas de anotação padrão do VEP usando `--everything`, maior uso dos cpus do gitpod `--fork 16` e diminuição do buffer de 5k para 1k `--buffer_size 200`.

```bash
docker run -it --rm  -v $(pwd):/data ensemblorg/ensembl-vep vep \
-i /data/WP312.filtered.vcf.gz  \
-o /data/vep_output/WP312.filtered.vep.vcf \
--assembly GRCh37  \
--merged \
--fork 16 \
--buffer_size 200 \
--force_overwrite \
--dir_cache /data/ \
--offline \
--cache \
--no_intergenic \
--distance 0 \
--pick \
--pick_allele \
--individual all \
--vcf \
--symbol \
--biotype \
--hgvs \
--numbers \
--af \
--af_gnomadg \
--variant_class \
--sift b \
--polyphen b \
--check_existing \
--fields "Location,SYMBOL,Consequence,Feature,BIOTYPE,HGVSc,HGVSp,EXON,INTRON,VARIANT_CLASS,SIFT,PolyPhen,AF,gnomADg_AF,CLIN_SIG,SOMATIC,PHENO" \
--fasta /data/homo_sapiens_merged/hg19.fa
```

### bcftools +split-vep



- git clone Htslib

```bash
git clone --recurse-submodules https://github.com/samtools/htslib.git
```

- git clone cftools

```bash
git clone https://github.com/samtools/bcftools.git
```

- entrar no diretório bcftools

```bash
cd bcftools/
```

- compilar

```bash
make
```

- testar

```bash
./bcftools
```

```bash
Program: bcftools (Tools for variant calling and manipulating VCFs and BCFs)
Version: 1.18-25-g44deedcd (using htslib 1.18-52-g2140d03e)

Usage:   bcftools [--version|--version-only] [--help] <command> <argument>

Commands:

 -- Indexing
    index        index VCF/BCF files

 -- VCF/BCF manipulation
    annotate     annotate and edit VCF/BCF files
    concat       concatenate VCF/BCF files from the same set of samples
    convert      convert VCF/BCF files to different formats and back
    head         view VCF/BCF file headers
    isec         intersections of VCF/BCF files
    merge        merge VCF/BCF files files from non-overlapping sample sets
    norm         left-align and normalize indels
    plugin       user-defined plugins
    query        transform VCF/BCF into user-defined formats
    reheader     modify VCF/BCF header, change sample names
    sort         sort VCF/BCF file
    view         VCF/BCF conversion, view, subset and filter VCF/BCF files

 -- VCF/BCF analysis
    call         SNP/indel calling
    consensus    create consensus sequence by applying VCF variants
    cnv          HMM CNV calling
    csq          call variation consequences
    filter       filter VCF/BCF files using fixed thresholds
    gtcheck      check sample concordance, detect sample swaps and contamination
    mpileup      multi-way pileup producing genotype likelihoods
    roh          identify runs of autozygosity (HMM)
    stats        produce VCF/BCF stats

 -- Plugins (collection of programs for calling, file manipulation & analysis)
    0 plugins available, run "bcftools plugin -l" for help

 Most commands accept VCF, bgzipped VCF, and BCF with the file type detected
 automatically even when streaming from a pipe. Indexed VCF and BCF will work
 in all situations. Un-indexed VCF and BCF and streams will work in most but
 not all situations.
```

- `export` - colocando o plugin do bcftools no path

```bash
export BCFTOOLS_PLUGINS=//workspace/somatico-vep/bcftools/plugins/
```






