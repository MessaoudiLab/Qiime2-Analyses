# **READ MAPPING**

## **Load Modules**

### **PROkaryotic DynamIc programming Genefinding ALgorithm**
```module load prodigal```

### **CD-hit is used for clustering and comparing protein or nucleotide sequences.** 
```module load cd-hit```

### **Bowtie2 is a short-read aligner.**
```module load bowtie2/2.2.9```

### **SAMTools is used for sequencing, aligning, and mapping.**
```module load samtools/1.9```

## **Locate File**

```ls *.fasta | sed 's/_1kb.fasta//' >Microbiome.prefixes.txt```

```ID=`head -n $SLURM_ARRAY_TASK_ID Microbiome.prefixes.txt | tail -n 1```

##  **Plug into Bowtie and create Samtools file**

```bowtie2 -q -p 40 -x ./all_samples/Concat_all_contigs_T1_non_redundant_good_header_index -U ../concat/T1/${ID}_concat.fastq.gz -S ${ID}.sam```

##  **Sort and prepare for viewing**

```samtools view -bu ${ID}.sam | samtools sort -T ${ID} -o ${ID}.sorted.bam -@ 48```

##  **Create file for extraction**

```samtools idxstats ${ID}.sorted.bam > ${ID}_mapping_stats_concat_index.txt```




# **NON-REDUNDANT AA**

## **Load modules**

### **PROkaryotic DynamIc programming Genefinding ALgorithm**
```module load prodigal```

### **CD-hit is used for clustering and comparing protein or nucleotide sequences.** 
```module load cd-hit```

### **Bowtie2 is a short-read aligner.**
```module load bowtie2/2.2.9```

## **Load fasta from document**

```ls *.fasta | sed 's/_1kb.fasta//' >Microbiome.prefixes.txt```

## **Begin task**

```ID=`head -n $SLURM_ARRAY_TASK_ID Microbiome.prefixes.txt | tail -n 1` ```

```cd-hit -i ${ID}.fna -d 0 -n 5 -l 100 -p 1 -G 0 -c 0.95 -aS 0.8 -M 0 -T 0 -o CD_non_redundant2```



# **CONCAT**

## **Load modules**

```module load prodigal```

```module load cd-hit```

```module load bowtie2/2.2.9```

## **Load fasta from document**

```ls *.fasta | sed 's/_1kb.fasta//' >Microbiome.prefixes.txt```

## **Begin task**

```ID=`head -n $SLURM_ARRAY_TASK_ID Microbiome.prefixes.txt | tail -n 1` ```

```bowtie2 -q -p 40 -x ./${ID}_non_redundant_index -U ../concat/${ID}_concat.fastq.gz -S ${ID}.sam```

