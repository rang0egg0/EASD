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

### Step 1.4: (slurm, PLINK) Create binary PLINK files for data
creates PLINK binary outputs from VCF and adds in phenotype (case/control status) data
#### Infile
EASD_CaseControl.goldenPath.decomposed.snpeff.snpsift.CC.ID.vcf.gz
#### Outfile
EASD.(bim, bed, fam, log, map, nosex, ped)
```
sbatch vcf_to_PLINK.slurm
```

### Step 1.5: (slurm, PLINK) Perform quality control on PLINK output
performs QC without regard to Hardy-Weinberg equilibrium and removes variants that do not meet QC standards
#### Infile
SCD.     
#### Outfile
SCD.QC.
```
cd PLINK
sbatch plinkQC.slurm
```

