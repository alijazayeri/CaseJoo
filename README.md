# CaseJoo
## CaseJoo: A Pipeline and R Library For Detection of Disease-causing Genes in Rare Genetic Disorders

This work has been presented as a poster at the 2019 ACMG Conference (S. Pajouhanfar, A. Jazayeri, M. Faghankhani, H. Vahidnezhad, L. Youssefian, S. Sotoudeh, S. Zeinali, J. Uitto, "CaseJoo: A Pipeline and R Library For Detection of Disease-causing Genes in Rare Genetic Disorders", 2019 ACMG Annual Clinical Genetics Meeting, Seattle, Washington).

The big picture is to find genes-diseases associations not very well-known already but indirectly and textually mentioned/discussed in the literature. 
CaseJoo prioritizes genes based on available papers for each gene in the `PubMed` and the similarity between the title/abstract of each paper and the clinical manifestations and/or phenotypes of the patient. 

The inputs are a set of genes and a set of keywords. The set of genes can be the outcome of a variant discovery pipeline, and the set of keywords are clinical manifestations of the patient in textual format. For example:

```R
sample_keys <- c('Ichthyosis','Palmoplantar hyperkeratosis','Anhidrosis','Erythroderma','Ectropion')
sample_genes <- c('ABCC1','DPYD', 'BCR', 'CDKN1C','CASQ2' ,'CYP1B1', 'ELOVL4','ITGA10' ,'KRT10','JTB' ,'KRT4','MRC1' ,'NIPA1', 'ORAI1','FBXL3' ,'PSORS1C1','RGL3' ,'RAD50','DDR1' ,'SQSTM1')
```

Then, using the `generate_query` function, a search query should be created. They query also can be directly generated in `PubMed` and used here:

```R
sample_query <- generate_query(sample_keys)
```

The created query with the list of unique genes should be sent to the `retrieve_genes` function. This function searches the `PubMed` for papers including the `gene` name in their titles/abstracts. This function returns a data frame composed of the list of genes with the number of papers retrieved for each gene. This list is generally a very smaller subset of the original list of genes and potentially includes the disease-causing gene. For searching the `PubMed`, `CaseJoo` uses `easyPubMed` library.

```R
sample_search <- retrieve_genes(sample_genes,sample_query)
```

Finally, the `prioritize_genes` function can be used for gene-wise calculation of similarity of papers for each gene and the keywords representing the clinical condition of the patient. 

```R
potential_gene <- sample_search$gene
sample_similarity <- prioritize_genes(potential_gene,sample_query,sample_keys)
```

This function returns a list composed of two data frames. The first data frame includes the rows corresponding to each paper retrieved successfully, the name of the related gene, and (cosine) TF-IDF-based similarity calculated between each paper and the keywords. The second data frame includes one row for each gene including the name of the gene and the mean of similarities grouped over all the papers related to each gene.

Finally, the outcome of the search can be visualized as follows:
<p align="center">
  <img src="https://github.com/alijazayeri/CaseJoo/blob/master/Sample_viz.png?raw=true">
</p>

In general, we can prioritize genes based on the following order:
- Genes with higher numbers of papers and higher values of similarity
- Genes with lower numbers of papers/higher values of similarity (or the other way around)
- Genes with lower numbers of papers/lower values of similarity
