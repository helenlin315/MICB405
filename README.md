# MICB405

## Shell script for Project 1

`#!/bin/bash`

`echo "Analyzing Variant Calling"`

`refPath=/projects/micb405/resources/project_1`

`for fastq in $refPATH/*_1.fastq.ga;`

`do`

  `echo -e "$fastq`
  
    `prefix=$(basename $fastq | sed 's/_1.fastq.gz//g')`
    
    `echo "Analyzing $prefix"`
    
    `bwa mem ~/VariantCalling2/ref_genome.fasta $refPATH/$prefix\_1.fastq.gz $refPATH/$prefix\_2.fastq.gz | \`
          
    `samtools view -b - > ~/VariantCalling2/$prefix.bam`
    
    `samtools sort ~/VariantCalling2/$prefix.bam -o ~/VariantCalling2/$prefix.sorted.bam`
    
    `samtools rmdup ~VariantCalling2/$prefix.sorted.bam ~/VariantCalling2/$prefix.sorted.rmdup.bam`
    
    `samtools index ~/VariantCalling2/$prefix.sorted
    
    `bcftools mpileup --fasta-ref ~/VariantCalling2/ref_genome.fasta ~VariantCalling2/$prefix.sorted.rmdup.bam \
         
         `| bcftools call -mv - >~/VariantCalling2/$prefix.vcf
         
`done`

`python /projects/micb405/resources/vcf_to_fasta_het.py -x ~/VariantCalling2/ finished_vcf`

`FastTree ~/VariantCalling2/finished_vcf.fasta > ~/VariantCalling2/finished_vcf.nwk`

`echo "finished"`


1. First copy the ref_genome file into my own directory
I created my own directory called VariantCalling2 in my home directory on the orca server. Then copied the reference over to my own directory. 

`cp /projects/micb/resources/project_1/ref_genome.fasta ~/VariantCalling2/`

2. Then I indexed my genome file using `bwa index`

`bwa index ~/VariantCalling2/ref_genome.fasta`

The outputs will be an indexed reference genome file, still in the fasta format

3. Then I started writing my 
