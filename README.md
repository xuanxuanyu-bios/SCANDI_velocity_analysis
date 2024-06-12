# SCANDI_velocity_analysis

## Summary of the data

The data contains 4 samples (S1_S4). Cell type annotation has been performed. We want to performed trajectory analysis to understanding the dynamic process and differentiation pathways of cells. The following steps are conducted to to the trajectory analysis

## Step 1 - Alignment and counting using CellRanger

Cell Ranger is a software suite provided by 10x Genomics designed to process and analyze single-cell RNA sequencing (scRNA-seq) data generated from their Chromium platform. It automates the workflow from raw sequencing reads to gene expression matrices and various quality control metrics. To process each sample separately:

```
#!/bin/bash
#SBATCH --job-name=SCANDI      # Job name
#SBATCH  --account=bercelis
#SBATCH  --qos=bercelis
#SBATCH --mail-type=NONE         # Mail events (NONE, BEGIN, END, FAIL, ALL)
#SBATCH --mail-user=xuanxuanyu@ufl.edu  # where to send mail.  Set this to your email address
#SBATCH --ntasks=1                  # Number of MPI tasks (i.e. processes)
#SBATCH --cpus-per-task=1            # Number of cores per MPI task
#SBATCH --mem=40gb                    # Job memory request
#SBATCH --time=48:00:00               # Time limit hrs:min:sec
#SBATCH --output=/blue/bercelis/share/Cellranger_SCANDI/output/-%j.log     # Path to the standard output and error files relative to the working directory
module load cellranger/8.0.0
cd /blue/bercelis/share/Cellranger_SCANDI/

# Array of sample names
samples=("SE7813_SA143825" "SE7813_SA143826" "SE7813_SA143827" "SE7813_SA143828")

OUTPUT_DIR="/blue/bercelis/share/Cellranger_SCANDI/Cellranger_output"
# Loop through each sample and run Cell Ranger
for sample in "${samples[@]}"; do
  cellranger count --id=$sample \
   --fastqs=/blue/bercelis/share/Cellranger_SCANDI/fastq \
   --sample=$sample \
   --transcriptome=/blue/bercelis/share/Cellranger_SCANDI/refdata-gex-GRCh38-2020-A/refdata-gex-GRCh38-2020-A \
   --create-bam true
  --output-dir=$OUTPUT_DIR/$sample
done

```
The cellranger output contains the required files for subsequent generation of ".loom" files which is needed to run the scvelo.


## Step 2: Generate .loom files using velocyto
This step can follow the <a href="https://github.com/xuanxuanyu-bios/SCANDI_velocity_analysis/blob/582a06f6c1b403a38e3810ebf79ecc77afebbc25/Tutorials/Share%20ef9c1a2e5ced4f86bacc799fc023e023.html" title="velocyto"> velocyto tutorial <\a>

