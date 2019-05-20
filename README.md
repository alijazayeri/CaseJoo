# CaseJoo
CaseJoo: A Pipeline and R Library For Detection of Disease-causing Genes in Rare Genetic Disorders
This work is presented as a poster presentation at ACMG 2019 Conference.

The big picture is to help finding genes-diseases associations not very well-known already but their association indirectly and textually mentioned in paper(s). 
CaseJoo prioritizes genes based on available papers for each gene in the `PubMed` and the similarity of the title/abstract of each gene's papers to the clinical manifestation and/or phenotypes of the patient. 

The inputs are a set of genes and a set of keywords. The set of genes can be the outcome of a variant discovery pipeline, and the set of keywords are clinical manifestations of the patient in the textual format. For example:

```R
sample_keys <- c('Ichthyosis','Palmoplantar hyperkeratosis','Anhidrosis','Erythroderma','Ectropion')
sample_genes <- c('ABCC1','DPYD', 'BCR', 'CDKN1C','CASQ2' ,'CYP1B1', 'ELOVL4','ITGA10' ,'KRT10','JTB' ,'KRT4','MRC1' ,'NIPA1', 'ORAI1','FBXL3' ,'PSORS1C1','RGL3' ,'RAD50','DDR1' ,'SQSTM1')
```


