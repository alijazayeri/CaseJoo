# CaseJoo
CaseJoo: A Pipeline and R Library For Detection of Disease-causing Genes in Rare Genetic Disorders

This work is presented as a poster presentation at the ACMG 2019 Conference.

The big picture is to find genes-diseases associations not very well-known already but their association indirectly and textually mentioned in the paper(s). 
CaseJoo prioritizes genes based on available papers for each gene in the `PubMed` and the similarity of the title/abstract of each gene's papers to the clinical manifestation and/or phenotypes of the patient. 

The inputs are a set of genes and a set of keywords. The set of genes can be the outcome of a variant discovery pipeline, and the set of keywords are clinical manifestations of the patient in the textual format. For example:

```R
sample_keys <- c('Ichthyosis','Palmoplantar hyperkeratosis','Anhidrosis','Erythroderma','Ectropion')
sample_genes <- c('ABCC1','DPYD', 'BCR', 'CDKN1C','CASQ2' ,'CYP1B1', 'ELOVL4','ITGA10' ,'KRT10','JTB' ,'KRT4','MRC1' ,'NIPA1', 'ORAI1','FBXL3' ,'PSORS1C1','RGL3' ,'RAD50','DDR1' ,'SQSTM1')
```

Then, using the `generate_query` function, a search query should be created:

```R
sample_query <- generate_query(sample_keys)
```

The created query with the list of unique genes should be sent to the `retrieve_genes` function. This function searches the `PubMed` for papers including the `gene` name in their titles/abstracts. This function returns a data frame composed of the list of genes with the number of papers retrieved for each gene. This list of generally is generally a very smaller subset of the original list of genes and potentially include the disease-causing gene. For searching the `PubMed`, `CaseJoo` uses `easyPubMed` library.

```R
sample_search <- retrieve_genes(sample_genes,sample_query)
```

Finally, the `prioritize_genes` function can be used for gene-wise calculation of similarity of papers for each gene and the keywords representing the clinical condition of the patient. 

```R
potential_gene <- sample_search$gene
sample_similarity <- prioritize_genes(potential_gene,sample_query,sample_keys)
```

This function returns a list of two data frame. The first data frame includes the rows corresponding to each paper retrieved successfully, the name of the related gene, and (cosine) TF-IDF-based similarity calculated between each paper and the keywords. The second data frame includes one row for each gene including the name of the gene and the mean of similarities grouped over all the papers related to the gene.

Finally, the outcome of the search can be visualized as follows:

<p align="center">
  <img width="300" height="300" src="(https://github.com/alijazayeri/CaseJoo/blob/master/Sample_viz.png">
</p>

In general, we can prioritize the genes as the following order:
- Genes with higher numbers of papers and with higher values of similarity
- Genes with lower numbers of papers/higher values of similarity (or the other way around)
- Genes with lower numbers of papers/lower values of similarity
