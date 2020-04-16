---
title: "Data exploration and properties"
teaching: 60
exercises: 0
questions:
- ""
objectives:
- ""
keypoints:
- ""
---

## Table of Contents
1. [Install and load the required packages](#install-and-load-the-required-packages)
2. [Import the data in R](#import-the-data-in-r)
3. [Create a phyloseq object](#create-a-phyloseq-object)
4. [Global exploration of the data sets](#global-exploration-of-the-data-sets)

## Install and load the required packages  

~~~
# install.packages("Vegan")     # only if you need to install the package
# install.packages("phyloseq")  # only if you need to install the package
# install.packages("tidyverse") # only if you need to install the package
# install.packages("patchwork") # only if you need to install the package

library(vegan)
library(phyloseq)
library(tidyverse)
library(patchwork)
~~~
{: .language-r}


## Import the data in R  

~~~
data_otu <- read.table("data_loue_16S_nonnorm.txt", header = TRUE)

data_grp <- read.table("data_loue_16S_nonnorm_grp.txt", header = TRUE)

data_taxo <- read.table("data_loue_16S_nonnorm_taxo.txt", header = TRUE)
~~~
{: .language-r}

The OTU table (`data_otu` or `data_loue_16S_nonnorm.txt`) is the occurence table that contains counts corresponding to the number of times each OTU (Operational Taxonomic Unit, here bacteria) is observed in each sample.  

The sample metadata table (`data_grp` or `data_loue_16S_nonnorm_grp.txt`) represents the metadata information on the different samples, in this case, where (*site*) and when (*month*) the samples were harvested.    

The taxonomy table (`data_taxo` or `data_loue_16S_nonnorm_taxo.txt`) represents the assignment for the different OTU at domain, phylum, class, order, family, genus and species levels. Usually, microbial ecologists use phylum level (or class level for Proteobacteria) to describe the global bacterial communities. Then, they usually use OTU, ASV (Amplicon Sequence Variant) or genus level to check which bacteria are differentially occuring in the different treatments.  

## Create a phyloseq object  
  
Microbial ecologists usually use the vegan and phyloseq packages to analyse the occurrence table. Such as *biom* format, phyloseq format support encapsulation of core study data (occurrence table data and sample/observation metadata) in a single file. From the three tables *data_otu*, *data_grp* and *data_taxo* we will create a phyloseq object *data_phylo*.  
  
~~~
OTU = otu_table(as.matrix(data_otu), taxa_are_rows = FALSE) # create the occurence table object in phyloseq format

SAM = sample_data(data_grp, errorIfNULL = TRUE) # create the sample metadata object in phyloseq format

TAX = tax_table(as.matrix(data_taxo)) # create the observation metadata object (OTU taxonomy) in phyloseq format

data_phylo <- phyloseq(OTU, TAX, SAM) # create the phyloseq object including occurrence table data and sample/observation metadata

data_phylo # print information about the phyloseq object
~~~~
{: .language-r}

## Global exploration of the data sets  
  
### OTU table (based on the raw data: *data_otu* table or *OTU* from *data_phylo* object)  
Check how the data look like showing the first 5 rows and the first 6 columns.  
~~~
data_otu[1:5, 1:6] # as we have here a high number of variables it is better here to not use head function for a better readability
~~~
{: .language-r}


The OTU table has samples in rows and variables (OTU) in columns.  

> ## Remark
> If you want to use phyloseq, you can do the same running `otu_table(data_phylo)[1:5, 1:6]` 
{: .callout}
  

Check how many samples and variables are in the OTU table?  
  
~~~
nb_samples <- dim(data_otu)[1] # nb of raws, here samples
nb_samples
nb_var <- dim(data_otu)[2] # number of columns, here variables (OTU)
nb_var
~~~
{: .language-r}


> ## Remark
> If you want to use phyloseq, you can use `nsamples()` and `ntaxa()` functions, such as `nsamples(data_phylo)` and `ntaxa(data_phylo)`.  
{: .callout}

### Sample metadata table 

`data_grp` table or `SAM` from `data_phylo` object.  

> ## Exercise
>
> How does the data look like if you show the first 6 rows using head function?   
> 
> > ## Solution
> > `# The sample metadata table has samples in row and factors in columns.`     
> > `head(data_grp)`
> {: .solution}
{: .challenge}  

> ## Remark
>If you want to use phyloseq, you can do the same running `head(sample_data(data_phylo))`.  
{: .callout}

> ## Exercise
>
> How many samples and factors are in the metadata table?   
> 
> > ## Solution
> > `dim(data_grp)[1] # number of samples.`     
> > `nb_factors <- dim(data_grp)[2] # number of factors`  
> > `nb_factors`
> {: .solution}
{: .challenge}  


  
Check how many samples per treatment do we have.  
~~~
summary(data_grp)
~~~
{: .language-r}
  
There are nine samples harvested or replicates (ignoring the month of harvest) for each of the two sites: Cleron (located at the upstream area of the river) and Parcey (located at the downstream area of the river). There are six replicates per month (ignoring the site of harvest). There are three replicates considering both site and month of harvest.  
  
> ## Remark
> If you want to use phyloseq, you can do the same running dim(sample_data(data_phylo))[1], dim(sample_data(data_phylo))[2] and summary(sample_data(data_phylo)$site)*  
{: .callout}
 
> ## Discussion point
>
> Question: This is a multivariate data set with an underlying experimental design. What would be your first idea to analyze this data?  
{: .discussion}

In the following part of this tutorial, we will discuss some problematic issues of these data that makes it necessary to use a specific strategy to deal with these data.  
  
  
  



  
  
