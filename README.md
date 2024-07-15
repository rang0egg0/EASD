# EASD - Case Control

### Step 1.1: (slurm, bcftools) Subset cases and controls from population VCF
#### Infile
joint_call_passing.annotated.goldenPath.20230726.vcf.gz
#### Outfile
EASD_CaseControl.goldenPath.vcf.gz 
```
sbatch subsetting_population_by_CasesControls.slurm
```

### Step 1.2: (slurm, SnpSift) CaseControl annotations
calculates four p-values for each variant based on four different models of genetic expression (dominant, recessive, codominant/genotypic, and Cochran Armitage). P-values are appended to the end of the INFO field
#### Infile
EASD_CaseControl.goldenPath.vcf.gz 
#### Outfile
EASD_CaseControl.goldenPath.decomposed.snpeff.snpsift.CC.vcf.gz
```
sbatch SnpSift_case_control.slurm
```

### Step 1.3 (slurm, BCFtools) Create unique IDs for SNPs
Annotates the "ID" column of the vcf to give each SNP a unique ID (chrom_pos_alt)
#### Infile
EASD_CaseControl.goldenPath.decomposed.snpeff.snpsift.CC.vcf.gz
#### Outfile
EASD_CaseControl.goldenPath.decomposed.snpeff.snpsift.CC.ID.vcf.gz
```
sbatch bcftools_generate_SNPids.slurm
tabix EASD_CaseControl.goldenPath.decomposed.snpeff.snpsift.CC.ID.vcf.gz
```

### Rename 
#### This to That
EASD_CaseControl.goldenPath.decomposed.snpeff.snpsift.CC.ID.vcf.gz ---> final.vcf.gz
##### Also i just copied data from the previous attempt
for some reason I couldnt get above scripts to work so 😭🤪🔫

### Step 1.4: (Python) Get type of variant
#### Infile
final.vcf.gz
#### Outfile 
SnpEff.hml.txt
```
ipython -i ~/Case_Control/scripts/get_type_of_variant.ipy -- -d ~/Case_Control/data/final.vcf.gz -p SnpEff

```
