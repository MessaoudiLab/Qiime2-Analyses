# **RUN PANPHLAN**

## **Activate source for modules**

```source activate Nick2.7```

## **Load modules**

### **SAMTools is used for sequencing, aligning, and mapping.**

```module load samtools```

### **BCFtools is a set of utilities that manipulate variant calls in the Variant Call Format (VCF) and its binary counterpart BCF.**

```module load bcftools```

### **Bowtie2 is a short-read aligner.**

```module load bowtie2```

## **Run humann2**
```humann2 -i ./${ID}.fastq.gz -o ./outhumann --nucleotide-database /rhome/rhoadesn/bigdata/chocophlan/ --protein-database /rhome/rhoadesn/bigdata/uniref50/ --metaphlan-options=--ignore_viruses --threads 60 --remove-temp-output --o-log ./outhumann/${ID}.txt metaphlan2.py ${ID}.fastq.gz --input_type fastq --samout ${ID}.sam.bz2 --nproc 40 --tax_lev t --sample_id ${ID}.strain.txt --ignore_viruses -o ../species/${ID}.strain.txt
metaphlan2.py -t marker_ab_table metagenome_outfmt.bz2 --input_type bowtie2out > marker_abundance_table.txt
metaphlan2.py -t marker_pres_table metagenome_outfmt.bz2 --input_type bowtie2out > marker_abundance_table.txt
metaphlan2.py -t clade_profiles ${ID}.bowtie2.bz2 --input_type bowtie2out > ${ID}_clade_table.txt metaphlan2.py -t clade_specific_strain_tracker --clade g__Prevotella ${ID}.bowtie2.bz2 --mpa_pkl /rhome/rhoadesn/bigdata/Shotgun_Metagenomics/RIPTIDE_4_30_19/combined/concat/concat/database/mpa_v20_m200.pkl --input_type bowtie2out > [$]{ID}_table.txt
metaphlan2.py ${ID}.bowtie2.bz2 --input_type bowtie2out  --nproc 40 --samout ${ID}.sam.bz2
sample2markers.py --ifn_samples ${ID}.sam.bz2 --input_type sam --output_dir ./strainphlan --nproc 40
sample2markers.py --ifn_samples ${ID}.sam.bz2 --input_type sam --output_dir ./strainphlan --nproc 20 --verbose --bcftools_exe /rhome/rhoadesn/bigdata/Shotgun_Metagenomics/RIPTIDE_4_30_19/combined/concat/concat/bcftools
python panphlan_map.py -i ../../../${ID}.fastq.gz --i_bowtie2_indexes ./ -c pstercorea16 -p 20 -o ${ID} --verbose```
