---
title: "ID mapping using mygene"
teaching: 7
exercises: 3
---

:::::::::::::::::::::::::::::::::::::: questions 

- What is Scikit-learn
- How is Scikit-learn used for a typical machine learning workflow?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explore the `sklearn` library.
- Walk through a typical machine learning workflow in Scikit-learn. 
- Examine machine learning evaluation plots.

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Scikit-learn is a popular package for machine learning in Python. 
- Scikit-learn has a variety of useful functionality for creating predictive models. 
- A machine learning workflow involves preprocessing, model selection, training, and evaluation. 

::::::::::::::::::::::::::::::::::::::::::::::::

## Installing mygene from bioconda

ID mapping is a very common, often not fun, task for every bioinformatician. 
Different datasets and tools often use different sets of biological identifiers.
Our goal is to take some list of gene symbols other gene identifiers and convert them to some other set of identifiers to facilitate some analysis.

**mygene** is a convenient Python module to access [MyGene.info](http://mygene.info) gene query web services.

Mygene is a part of a family of [biothings API](https://biothings.io/) services which also has modules for chemical, diseases, genetic variances, and taxonomy. 
All of these modules use a simlar Python interface.


Mygene can be installed using conda, but we it exists on a separate set of biomedical packages called [bioconda](https://bioconda.github.io/).
We access this using the `-c` flag which tells conda the *channel* we wish to search.

```bash
$ conda install -c bioconda mygene
```

Once installed, we can import `mygene` and create a `MyGeneInfo` object, which is used for accessing and downloading genetic annotations. 

```python
import mygene

mg = mygene.MyGeneInfo()
```

### Mapping gene symbols

Suppose **xli** is a list of gene symbols we want to convert to entrez gene ids:

```python
xli = ['DDX26B',
'CCDC83',
'MAST3',
'FLOT1',
'RPL11',
'ZDHHC20',
'LUC7L3',
'SNORD49A',
'CTSH',
'ACOT8']
```

We can then use the **querymany** method to get entrez gene ids for this list, telling it your input is "symbol" with the `scopes` argument, and you want "entrezgene" (Entrez gene ids) back with the `fields` argument.

```python
out = mg.querymany(xli, scopes='symbol', fields='entrezgene', species='human')
print(out)
```

```output
querying 1-10...done.
Finished.
1 input query terms found no hit:
	['DDX26B']
Pass "returnall=True" to return complete lists of duplicate or missing query terms.
[{'query': 'DDX26B', 'notfound': True}, {'query': 'CCDC83', '_id': '220047', '_score': 18.026102, 'entrezgene': '220047'}, {'query': 'MAST3', '_id': '23031', '_score': 18.120222, 'entrezgene': '23031'}, {'query': 'FLOT1', '_id': '10211', '_score': 18.43404, 'entrezgene': '10211'}, {'query': 'RPL11', '_id': '6135', '_score': 16.711576, 'entrezgene': '6135'}, {'query': 'ZDHHC20', '_id': '253832', '_score': 18.165747, 'entrezgene': '253832'}, {'query': 'LUC7L3', '_id': '51747', '_score': 17.696375, 'entrezgene': '51747'}, {'query': 'SNORD49A', '_id': '26800', '_score': 22.741982, 'entrezgene': '26800'}, {'query': 'CTSH', '_id': '1512', '_score': 17.737492, 'entrezgene': '1512'}, {'query': 'ACOT8', '_id': '10005', '_score': 17.711779, 'entrezgene': '10005'}]
```

The mapping result is returned as a list of dictionaries. 
Each dictionary contains the **fields** we asked to return, in this case, "entrezgene" field. 
Each dictionary also returns the matching query term, "**query**", and an internal id, "**_id**", which is the same as "**entrezgene**" most of time (will be an ensembl gene id if a gene is available from Ensembl only).
Mygene could not find a mapping for one of the genes. 

We could also get Ensembl gene ids. 
This time, we will use the `as.dataframe` argument to get the result back as a pandas dataframe. 

```python
ensembl_res = mg.querymany(xli, scopes='symbol', fields='ensembl.gene', species='human', as_dataframe=True)
print(ensembl_res)
```

```output
querying 1-10...done.
Finished.
1 input query terms found no hit:
	['DDX26B']
Pass "returnall=True" to return complete lists of duplicate or missing query terms.
         notfound     _id     _score     ensembl.gene  \
query                                                   
DDX26B       True     NaN        NaN              NaN   
CCDC83        NaN  220047  18.026253  ENSG00000150676   
MAST3         NaN   23031  18.120222  ENSG00000099308   
FLOT1         NaN   10211  18.416580              NaN   
RPL11         NaN    6135  16.711576  ENSG00000142676   
ZDHHC20       NaN  253832  18.165747  ENSG00000180776   
LUC7L3        NaN   51747  17.701532  ENSG00000108848   
SNORD49A      NaN   26800  22.728123  ENSG00000277370   
CTSH          NaN    1512  17.725145  ENSG00000103811   
ACOT8         NaN   10005  17.717607  ENSG00000101473   

                                                    ensembl  
query                                                        
DDX26B                                                  NaN  
CCDC83                                                  NaN  
MAST3                                                   NaN  
FLOT1     [{'gene': 'ENSG00000206480'}, {'gene': 'ENSG00...  
RPL11                                                   NaN  
ZDHHC20                                                 NaN  
LUC7L3                                                  NaN  
SNORD49A                                                NaN  
CTSH                                                    NaN  
ACOT8                                                   NaN 
```

### Mixed symbols and multiple fields

```python
xli = ['ENSG00000150676',
 'MAST3',
 'FLOT1',
 'RPL11',
 '1007_s_at',
 'AK125780']
 ```

The above id list contains multiple types of identifiers. 
Let's try taking this list and trying to get back both Entrez gene ids and uniprot ids. Parameters **scopes**, **fields**, **species** are all flexible enough to support multiple values, either a list or a comma-separated string.

```python
query_res = mg.querymany(xli, scopes='symbol,reporter,accession,ensembl.gene', fields='entrezgene,uniprot', species='human', as_dataframe=True)
print(query_res)
```

```output
querying 1-6...done.
Finished.
1 input query terms found dup hits:
	[('1007_s_at', 2)]
Pass "returnall=True" to return complete lists of duplicate or missing query terms.
                       _id     _score entrezgene uniprot.Swiss-Prot  \
query                                                                 
ENSG00000150676     220047  24.495731     220047             Q8IWF9   
MAST3                23031  18.115461      23031             O60307   
FLOT1                10211  18.416580      10211             O75955   
RPL11                 6135  16.715294       6135             P62913   
1007_s_at        100616237  18.266214  100616237                NaN   
1007_s_at              780  18.266214        780             Q08345   
AK125780         118142757  21.342941  118142757             P43080   

                                                    uniprot.TrEMBL  
query                                                               
ENSG00000150676                                             H0YDV3  
MAST3            [V9GYV0, A0A8V8TLL8, A0A8I5KST9, A0A8V8TMW0, A...  
FLOT1            [A2AB11, Q5ST80, A2AB13, A2AB10, A2ABJ5, A2AB1...  
RPL11                                 [Q5VVC8, Q5VVD0, A0A2R8Y447]  
1007_s_at                                                      NaN  
1007_s_at        [A0A024RCJ0, A0A024RCL1, A0A0A0MSX3, A0A024RCQ...  
AK125780                                      [A0A7I2V6E2, B2R9P6]  
```

Finally, we can take a look at all of the information mygene has to offer by setting `fields` to "all".


```python
query_res = mg.querymany(xli, scopes='symbol,reporter,accession,ensembl.gene', fields='all', species='human', as_dataframe=True)
print(list(query_res.columns))
```

```output
querying 1-6...done.
Finished.
1 input query terms found dup hits:
	[('1007_s_at', 2)]
Pass "returnall=True" to return complete lists of duplicate or missing query terms.
['AllianceGenome', 'HGNC', '_id', '_score', 'alias', 'entrezgene', 'exons', 'exons_hg19', 'generif', 'ipi', 'map_location', 'name', 'other_names', 'pharmgkb', 'symbol', 'taxid', 'type_of_gene', 'unigene', 'accession.genomic', 'accession.protein', 'accession.rna', 'accession.translation', 'ensembl.gene', 'ensembl.protein', 'ensembl.transcript', 'ensembl.translation', 'ensembl.type_of_gene', 'exac._license', 'exac.all.exp_lof', 'exac.all.exp_mis', 'exac.all.exp_syn', 'exac.all.lof_z', 'exac.all.mis_z', 'exac.all.mu_lof', 'exac.all.mu_mis', 'exac.all.mu_syn', 'exac.all.n_lof', 'exac.all.n_mis', 'exac.all.n_syn', 'exac.all.p_li', 'exac.all.p_null', 'exac.all.p_rec', 'exac.all.syn_z', 'exac.bp', 'exac.cds_end', 'exac.cds_start', 'exac.n_exons', 'exac.nonpsych.exp_lof', 'exac.nonpsych.exp_mis', 'exac.nonpsych.exp_syn', 'exac.nonpsych.lof_z', 'exac.nonpsych.mis_z', 'exac.nonpsych.mu_lof', 'exac.nonpsych.mu_mis', 'exac.nonpsych.mu_syn', 'exac.nonpsych.n_lof', 'exac.nonpsych.n_mis', 'exac.nonpsych.n_syn', 'exac.nonpsych.p_li', 'exac.nonpsych.p_null', 'exac.nonpsych.p_rec', 'exac.nonpsych.syn_z', 'exac.nontcga.exp_lof', 'exac.nontcga.exp_mis', 'exac.nontcga.exp_syn', 'exac.nontcga.lof_z', 'exac.nontcga.mis_z', 'exac.nontcga.mu_lof', 'exac.nontcga.mu_mis', 'exac.nontcga.mu_syn', 'exac.nontcga.n_lof', 'exac.nontcga.n_mis', 'exac.nontcga.n_syn', 'exac.nontcga.p_li', 'exac.nontcga.p_null', 'exac.nontcga.p_rec', 'exac.nontcga.syn_z', 'exac.transcript', 'genomic_pos.chr', 'genomic_pos.end', 'genomic_pos.ensemblgene', 'genomic_pos.start', 'genomic_pos.strand', 'genomic_pos_hg19.chr', 'genomic_pos_hg19.end', 'genomic_pos_hg19.start', 'genomic_pos_hg19.strand', 'go.MF.category', 'go.MF.evidence', 'go.MF.id', 'go.MF.pubmed', 'go.MF.qualifier', 'go.MF.term', 'homologene.genes', 'homologene.id', 'interpro.desc', 'interpro.id', 'interpro.short_desc', 'pantherdb.HGNC', 'pantherdb._license', 'pantherdb.ortholog', 'pantherdb.uniprot_kb', 'pharos.target_id', 'reagent.GNF_Qia_hs-genome_v1_siRNA', 'reagent.GNF_hs-ORFeome1_1_reads.id', 'reagent.GNF_hs-ORFeome1_1_reads.relationship', 'reagent.GNF_mm+hs-MGC.id', 'reagent.GNF_mm+hs-MGC.relationship', 'reagent.GNF_mm+hs_RetroCDNA.id', 'reagent.GNF_mm+hs_RetroCDNA.relationship', 'reagent.NOVART_hs-genome_siRNA', 'refseq.genomic', 'refseq.protein', 'refseq.rna', 'refseq.translation', 'reporter.GNF1H', 'reporter.HG-U133_Plus_2', 'reporter.HTA-2_0', 'reporter.HuEx-1_0', 'reporter.HuGene-1_1', 'reporter.HuGene-2_1', 'umls.cui', 'uniprot.Swiss-Prot', 'uniprot.TrEMBL', 'MIM', 'ec', 'interpro', 'pdb', 'pfam', 'prosite', 'summary', 'go.BP', 'go.CC.evidence', 'go.CC.gocategory', 'go.CC.id', 'go.CC.qualifier', 'go.CC.term', 'go.MF', 'reagent.GNF_hs-Origene.id', 'reagent.GNF_hs-Origene.relationship', 'reagent.GNF_hs-druggable_lenti-shRNA', 'reagent.GNF_hs-druggable_plasmid-shRNA', 'reagent.GNF_hs-druggable_siRNA', 'reagent.GNF_hs-pkinase_IDT-siRNA', 'reagent.Invitrogen_IVTHSSIPKv2', 'reagent.NIBRI_hs-Secretome_pDEST.id', 'reagent.NIBRI_hs-Secretome_pDEST.relationship', 'reporter.HG-U95Av2', 'ensembl', 'genomic_pos', 'genomic_pos_hg19', 'go.CC', 'pathway.kegg.id', 'pathway.kegg.name', 'pathway.netpath.id', 'pathway.netpath.name', 'pathway.reactome', 'pathway.wikipathways', 'reagent.GNF_hs-Origene', 'wikipedia.url_stub', 'pir', 'pathway.kegg', 'pathway.pid', 'pathway.wikipathways.id', 'pathway.wikipathways.name', 'miRBase', 'retired', 'reagent.GNF_mm+hs-MGC'
```

You can find a neater version of all available fields [here](https://docs.mygene.info/en/latest/doc/data.html#available-fields).

::: challenge

Load in the rnaseq dataset as a pandas dataframe. 
Add columns to the dataframe with entrez gene ids and at least one other field from mygene.

:::

## Other bioinformatics resources

[Biopython](https://biopython.org/) is a library with a wide variety of bioinformatics applications.
You can find its tutorial/cookbook [here](https://biopython.org/DIST/docs/tutorial/Tutorial.html)


---

*This lesson was adapted from the [official mygene python tutorial](https://nbviewer.org/gist/newgene/6771106)*