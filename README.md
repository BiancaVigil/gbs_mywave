## GBS
**APLICACIONES**
<div>
<p style = 'text-align:center;'>
<img src="Applications-of-Genotyping-by-Sequencing-integrating-broad-areas-of-genetics-and.png" width="1000px">
</p>
</div>

## PIPELINE GBS-PROCESING
<div>
<p style = 'text-align:center;'>
<img src="pipeline_GBS.png" width="1000px">
</p>
</div>


## [STACKS](https://catchenlab.life.illinois.edu/stacks/)

[Descargar](https://catchenlab.life.illinois.edu/stacks/)

[Instalar-manual](https://catchenlab.life.illinois.edu/stacks/manual/#install)

<div>
<p style = 'text-align:center;'>
<img src="stacks_full_pipelines.png" width="700px">
</p>
</div>

## DEMULTIPLEXACIÓN - FILTRADO - STACKS

[**process_radtags**](https://catchenlab.life.illinois.edu/stacks/manual-v1/)

1.RawData
<div>
<p style = 'text-align:center;'>
<img src="rawdata.png" width="600px">
</p>
</div>
2.Barcodes para cada individuo
<div>
<p style = 'text-align:center;'>
<img src="barcodes.png" width="200px">
</p>
</div>
3.Demultiplexado o individualización
<div>
<p style = 'text-align:center;'>
<img src="demultiplexado.png" width="400px">
</p>
</div>

## ALINEAMIENTO A GENOMA DE REFERENCIA
1.BWA - BORROWS WHEELER ALIGNER
```{}
sudo apt-get update
sudo apt-get install bwa
```
2.INDEXADO DEL GENOMA DE REFERENCIA
```{}
#!/bin/bash
src="/home/bvigil/GBS_SEMINARIO/genoma"
bwa index -p manihot-index -a bwtsw $src/GCF_001659605.2_M.esculenta_v8_genomic.fna
```
3.ALINEAMIENTO 
```{}
#!/bin/bash
src="/home/bvigil/GBS_SEMINARIO/"
bwa_db="/home/bvigil/GBS_SEMINARIO/genoma/manihot-index"

files="ME_038_S9_R1_001
ME_094_S10_R1_001
ME_171_S11_R1_001
ME_272_S12_R1_001
ME_368_S13_R1_001
ME_590_S14_R1_001
ME_674_S15_R1_001
ME_681_S16_R1_001"

for ME in $files
do
        bwa mem -t 12 $bwa_db $src/data/${ME}.fastq.gz > $src/alineamiento/${ME}.sam
done

```
4.SAM --> BAM
```{}
#!/bin/bash
src="/home/bvigil/GBS_SEMINARIO/"

files="ME_038_S9_R1_001
ME_094_S10_R1_001
ME_171_S11_R1_001
ME_272_S12_R1_001
ME_368_S13_R1_001
ME_590_S14_R1_001
ME_674_S15_R1_001
ME_681_S16_R1_001"

for ME in $files
do
        samtools view -b -S $src/alineamiento/${ME}.sam -o $src/alineamiento/${ME}.bam
done
```
5.SORT 
```{}
#!/bin/bash
src="/home/bvigil/GBS_SEMINARIO/alineamiento/"

files="ME_038_S9_R1_001
ME_094_S10_R1_001
ME_171_S11_R1_001
ME_272_S12_R1_001
ME_368_S13_R1_001
ME_590_S14_R1_001
ME_674_S15_R1_001
ME_681_S16_R1_001"

for ME in $files
do
        samtools sort $src/${ME}.bam -o $src/sort/${ME}.bam
done
```

## GSTACKS-STACKS
```{}
#!/bin/bash

src="/home/bvigil/GBS_SEMINARIO"
gstacks -I $src/alineamiento/sort/ -M $src/scripts/popmap-yuca-piloto -O $src/stacks -t 24
```

## POPULATIONS - VCF
```{}
#!/bin/bash

src="/home/bvigil/GBS_SEMINARIO"
populations -P $src/stacks/ -M $src/scripts/popmap-yuca-piloto --vcf --plink -t 24
```

## FORMATO [VCF](https://samtools.github.io/hts-specs/VCFv4.2.pdf)

VCF es un formato de archivo de texto (lo más probable es que se almacene de forma comprimida). Contiene líneas de metainformación, una línea de encabezado y luego líneas de datos, cada una de las cuales contiene información sobre una posición en el genoma. El formato también tiene la capacidad de contener información de genotipo en muestras para cada posición.

<div>
<p style = 'text-align:center;'>
<img src="VCF_FORMAT.png" width="1000px">
</p>
</div>

## PROGRAMAS - SNP CALLING
1. SAMTOOLS 
```{}
sudo apt-get update
sudo apt-get install samtools
```
2. VCFTOOLS
```{}
sudo apt-get update
sudo apt-get install vcftools
```
3. BCFTOOLS
```{}
sudo apt-get update
sudo apt-get install bcftools
```


## DESCARGAR ARCHIVO

Descargar [archivo VCF](https://drive.google.com/drive/folders/18xm4UcUeKhOCNCGLsWXVWW3jCQtKgwoV?usp=sharing)
