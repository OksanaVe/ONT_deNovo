This directory contains information about de novo genome assembly steps.  
We will be using Flye, Canu, MetaMDBG, and HaploFlow for metagenome assemblies.  

## To-do list  

- [ ] Perform assemblies under the default settings for each assembler (for Canu, we'll use settings recommended for metagenomics assemblies, not the defaults);  
- [ ] Visualize assembly graphs in Bandage;  
- [ ] Classify assembled contigs using BLAST search;  


## Flye  
GitHub: https://github.com/fenderglass/Flye/tree/flye  
Example of simple bash script to run Flye in 'meta' mode:  

```
#!/bin/bash  
FILES="./*.fq.gz"  

for f in $FILES  
do  
s=${f##*/}  

flye --nano-corr ${f} --threads 52 --meta --min-overlap 1000 --out-dir "${s%_concat*}_metaFlye"  

done
```

## Canu  
GitHub: https://github.com/marbl/canu  

```
#!/bin/bash
FILES="./*.fq.gz"

for f in $FILES
do
s=${f##*/}

canu -d "${s%.fq.gz*}_canu_5m" -p "canu_NanoSim_${s%.fq.gz*}_5m" corOutCoverage=10000 corMhapSensitivity=high corMinCoverage=0 redMemory=32 oeaMemory=32 batMemory=200 correctedErrorRate=0.105 genomeSize=5m corMaxEvidenceCoverageLocal=10 corMaxEvidenceCoverageGlobal=10 -corrected -trimmed -nanopore ${f}

done
```

## MetaMDBG  
GitHub: https://github.com/GaetanBenoitDev/metaMDBG  
Best to install from source using conda/mamba (if installing directly with conda, assembly graph function won't work correctly)    
Run assembler under the default settings first:  
```
metaMDBG asm path/to/output_directory reads_concat.fq.gz -t 52
```
After assembly is done, generate an assembly graph:  
```
metaMDBG gfa assemblyDir k --contigpath --readpath
```  

## HaploFlow  
GitHub: https://github.com/hzi-bifo/Haploflow  

## Bandage  
Visualize assembly graphs using Bandage: https://rrwick.github.io/Bandage/
