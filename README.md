# Biocartograph
Creating Cartographic Representations of Biological Data
[![DOI](https://zenodo.org/badge/578172132.svg)](https://zenodo.org/badge/latestdoi/578172132)

# Installation
```
pip install biocartograph
```

# Example code
```
if __name__ == '__main__' :
    from biocartograph.quantification import full_mapping
    #
    adf = pd.read_csv('analytes.tsv',sep='\t',index_col=0)
    #
    # WE DO NOT WANT TO KEEP POTENTIALLY BAD ENTRIES 
    adf = adf.iloc[ np.inf != np.abs( 1.0/np.std(adf.values,1) ) ,
                    np.inf != np.abs( 1.0/np.std(adf.values,0) ) ].copy()
    #
    # READING IN SAMPLE INFORMATION
    # THIS IS NEEDED FOR THE ALIGNED PCA TO WORK
    jdf = pd.read_csv('journal.tsv',sep='\t',index_col=0)
    jdf = jdf.loc[:,adf.columns.values]
    #
    alignment_label , sample_label = 'Disease' , None
    add_labels = ['Cell-line']
    #
    cmd                = 'max'
    # WRITE FILES AND MAKE NOISE
    bVerbose           = True
    # CREATE AN OPTIMIZED REPRESENTATION
    bExtreme           = True
    # WE MIGHT WANT SOME SPECIFIC INTERSECTIONS OF THE HIERARCHY
    n_clusters         = [20,40,60,80,100]
    # USE ALL INFORMATION
    n_components       = None
    umap_dimension     = 2
    n_neighbors        = 20
    local_connectivity = 20.
    transform_seed     = 42
    #
    print ( adf , jdf )
    #
    # distance_type = 'correlation,spearman,absolute' # DONT USE THIS
    distance_type = 'covariation' # BECOMES CO-EXPRESSION BASED
    #
    results = full_mapping ( adf , jdf                  ,
        bVerbose = bVerbose             ,
        bExtreme = bExtreme             ,
        n_clusters = n_clusters         ,
        n_components = n_components     ,
        distance_type = distance_type   ,
        umap_dimension = umap_dimension ,
        umap_n_neighbors = n_neighbors  ,
        umap_local_connectivity = local_connectivity ,
        umap_seed = transform_seed      ,
        hierarchy_cmd = cmd             ,
        add_labels = add_labels         ,
        alignment_label = alignment_label ,
        sample_label = None     )
    #
    map_analytes        = results[0]
    map_samples         = results[1]
    hierarchy_analytes  = results[2]
    hierarchy_samples   = results[3]
```
or just call it using the default values:
```
import pandas as pd
import numpy  as np

if __name__ == '__main__' :
    from biocartograph.quantification import full_mapping
    #
    adf = pd.read_csv('analytes.tsv',sep='\t',index_col=0)
    #
    adf = adf.iloc[ np.inf != np.abs( 1.0/np.std(adf.values,1) ) ,
                    np.inf != np.abs( 1.0/np.std(adf.values,0) ) ].copy()
    jdf = pd.read_csv('journal.tsv',sep='\t',index_col=0)
    jdf = jdf.loc[:,adf.columns.values]
    #
    alignment_label , sample_label = 'Disease' , None
    add_labels = ['Cell-line']
    #
    results = full_mapping ( adf , jdf  ,
        bVerbose = True			,
        n_clusters = [40,80,120]        ,
        add_labels = add_labels         ,
        alignment_label = alignment_label )
    #
    map_analytes        = results[0]
    map_samples         = results[1]
    hierarchy_analytes  = results[2]
    hierarchy_samples   = results[3]
```
and plotting the information of the map analytes yields :
[Cancer Disease Example](https://gist.github.com/rictjo/9cc40579914a51bffe7df442fec140f4)

You can also run an alternative algorithm where the UMAP coordinates are employed directly for clustering by setting
```
    results = full_mapping ( adf , jdf  ,
        bVerbose = True			        ,
        bUseUmap = True                 ,
        n_clusters = [40,80,120]        ,
        add_labels = add_labels         ,
        alignment_label = alignment_label )
```
with the following [results](https://rictjo.github.io/?https://gist.githubusercontent.com/rictjo/8be5b5a9cc7f06ea7455d6c6ecc11ad8/raw/e00ea663a1218718f542744a939e0b05c604e8ab/index.html).

Download the zip and open the html index:
```
chromium index.html
```

# Other generated solutions

The clustering visualisations were created using the [Biocartograph](https://pypi.org/project/biocartograph/) and [hvplot](https://pypi.org/project/hvplot/) :

What groupings corresponds to biomarker variance that describe them? Here are two visualisations of that:

Diseases :
[cancers](https://rictjo.github.io/?https://gist.githubusercontent.com/rictjo/870d8cc26ede12d00b7ae60109feebdc/raw/42beb98a82477e9c809f99d3498966fc564846b8/index.html) [biocartograph gfa Reactome enrichments](https://rictjo.github.io/?https://gist.githubusercontent.com/rictjo/afcca63470e5c9398372276f9ab43d42/raw/6e68e1da85fdb6d1b1aeec8c351831a3aad83e9d/index.html) [biocartograph gfa cluster enrichments](https://rictjo.github.io/?https://gist.githubusercontent.com/rictjo/5d83a85537839232f34edccde1cdc8e6/raw/40c49013a55213405a6b6609f9ab31c883668d5d/index.html)
[biocartograph treemap cluster 61](https://rictjo.github.io/?https://gist.githubusercontent.com/rictjo/146ba66109c6554684dc387348d21a82/raw/a32f1e7c80cc6ebe53c33039e2adfb4512e3ce4b/index.html)

Tissues :
[tissues](https://rictjo.github.io/?https://gist.githubusercontent.com/rictjo/5e760b8c4fd3da4842813a4a0cea422c/raw/caa18f0391dc389fb8fc56ae8ac2bc4f7046a939/index.html)

Single Cells:
[single cells](https://rictjo.github.io/?https://gist.githubusercontent.com/rictjo/eb118f70c1d173f2e6d51f06779827d2/raw/c7fd997caf232df3d6bbbd80d607463812d461a1/index.html) [biocartograph gfa enrichment](https://rictjo.github.io/?https://gist.githubusercontent.com/rictjo/64ee6e4d2bacb31715ec46b65c9d441d/raw/a5d91114cc4ab784f865277264efe5f628ea018e/index.html) [biocartograph treemap cluster 47](https://rictjo.github.io/?https://gist.githubusercontent.com/rictjo/34b320ea503b79e29808b063a7266714/raw/eaf39e740eb8baaadf0d08faab521a152c282009/index.html)

Blood Cells:
[blood cells](https://rictjo.github.io/?https://gist.githubusercontent.com/rictjo/200153c58767d8b5162e66688ff4d669/raw/cfb74069d5cc9fc58e3558c753caaa60d4ba5e9b/index.html)
[biocartograph gfa enrichment](https://rictjo.github.io/?https://gist.githubusercontent.com/rictjo/42ec85df088a0c40de339a78322594bd/raw/0725bea467b0c153298655e3a0555670a812e80f/index.html)
[biocartograph treemap cluster 2](https://rictjo.github.io/?https://gist.githubusercontent.com/rictjo/d754528cf594087e509fe44fa071c178/raw/a78a82066e3d6aa2971aba2a64543a4018241372/index.html)
