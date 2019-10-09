# OpenPREDICT: Open and FAIR implementation of the PREDICT method for drug repurposing

Open and FAIR implementation of the PREDICT method described in the paper titled "PREDICT: a method for inferring novel drug indications with application to personalized medicine.", Gottlieb A, Stein GY, Ruppin E, Sharan R. Mol Syst Biol. 2011;7:496. Published 2011 Jun 7. doi:10.1038/msb.2011.26

## Sources
-------

### Bio2RDF datasets
| Dataset | RDF Dataset Obtained from | Metadata |
|-------------| ------------- | ------------- |
| **Drugbank**| http://download.bio2rdf.org/files/release/4/drugbank/drugbank.nq.gz | http://download.bio2rdf.org/files/release/4/drugbank/bio2rdf-drugbank.nq |
| **Kegg** | http://download.bio2rdf.org/files/release/4/kegg/kegg-drug.nq.gz \\ http://download.bio2rdf.org/files/release/4/kegg/kegg-genes.nq.gz | http://download.bio2rdf.org/files/release/4/kegg/bio2rdf-kegg.nq |
|**SIDER** | http://download.bio2rdf.org/files/release/4/sider/sider-se.nq.gz | http://download.bio2rdf.org/files/release/4/sider/bio2rdf-sider.nq |
| **HGNC** | http://download.bio2rdf.org/files/release/4/hgnc/hgnc.nq.gz | http://download.bio2rdf.org/files/release/4/hgnc/bio2rdf-hgnc.nq |
| **GOA** | http://download.bio2rdf.org/files/release/4/goa/goa_human.nq.gz | http://download.bio2rdf.org/files/release/4/goa/bio2rdf-goa.nq |

### FAIRified datasets
| Dataset | Raw Data| RDF Data Generated  | Metadata | Generated By |
|-------------| ------------- | ------------- |  ------------- | ------------- |
| **PREDICT drug indication gold standard**| [msb201126-s1.csv](data/external/msb201126-s1.csv) | [predict_gold_standard_omim.nq.gz](data/rdf/predict_gold_standard_omim.nq.gz) | [predict_gold_standard_omim_metadata.nq](data/rdf/predict_gold_standard_omim_metadata.nq) | [MappingPREDICTGoldstandard.ipynb](MappingPREDICTGoldstandard.ipynb) |
| **Pubchem IDs mapping for Drugbank** | [pubchem.tsv](data/external/pubchem.tsv) | [pubchem_mapping.nq.gz](data/rdf/pubchem_mapping.nq.gz) | [pubchem_mapping_metadata.nq](data/rdf/pubchem_mapping_metadata.nq) | [RDFConversionOfPubchemMapping.ipynb](RDFConversionOfPubchemMapping.ipynb) |
| **Protein-protein interactions** | [human_interactome.tsv](data/external/human_interactome.tsv) | [human_interactome.nq.gz](data/rdf/human_interactome.nq.gz) | [human_interactome_metadata.nq](data/rdf/human_interactome_metadata.nq) | [HumanInteractome.ipynb](HumanInteractome.ipynb) |
| **HPO Phenotype annotations for diseases** | [phenotype_annotation_hpoteam.tab](external/phenotype_annotation_hpoteam.tab) | [hpo_annotations.nq.gz](data/rdf/hpo_annotations.nq.gz) | [hpo_annotations_metadata.nq](data/rdf/hpo_annotations_metadata.nq) | [OMIMHpoAnnotations.ipynb](OMIMHpoAnnotations.ipynb)| 
| **MESH Phenotype annotations for diseases** |[mim2mesh.tsv](data/external/mim2mesh.tsv) | [omim_mesh_annotations.nq.gz](data/rdf/omim_mesh_annotations.nq.gz) | [omim_mesh_annotations_metadata.nq](data/rdf/omim_mesh_annotations_metadata.nq) | [RDFConversionOfMeshAnnotation.ipynb](RDFConversionOfMeshAnnotation.ipynb) |
| **MESH Phenotype annotations using BioPortal for diseases** | [meshAnnotationsFromBioPorttalUsingOMIMDesc.txt](data/external/meshAnnotationsFromBioPorttalUsingOMIMDesc.txt) | [omim_mesh_bioportal.nq.gz](data/rdf/omim_mesh_bioportal.nq.gz) | [omim_mesh_bioportal_metadata.nq](data/rdf/omim_mesh_bioportal_metadata.nq) | [RDFConversionOfMeshAnnotation-BioPortal.ipynb](RDFConversionOfMeshAnnotation-BioPortal.ipynb) and raw file generated by [getMeshTerms.py](src/getMeshTerms.py )|


## Pre-processing Bio2RDF data
This issue is related to Bio2RDF datasets. The format of each Bio2RDF dataset has to be fixed before uploading it to the triple-store.
Example:
```bash
python src/preprocess_bio2rdf.py -i drugbank.nq.gz -o refined_drugbank.nq.gz
```

## Upload RDF data to the triple store
Upload each RDF data into triple-store (GraphDB or Virtuoso)


## Requirement
```bash
docker
python 3.6
pip install networkx==1.11
pip install numpy==1.15.4
pip install pandas==0.24.2
pip install sklearn==0.20.3
pip install rdflib==4.2.2
conda install -c bioconda cwltool
conda install -c oddt oddt
conda install -c openbabel openbabel
```


## How to reproduce the results?  
* Use the OpenPREDICT GraphDB SPARQL endpoint (http://graphdb.dumontierlab.com/repositories/openpredict) to query all data
* If you don't want to use the given SPARQL endpoint, collect all sources from given links and pre-process bio2rdf datasets (see section: Pre-processing Bio2RDF data ), then create your triple store and upload each RDF data into your triple store (currently tested with GraphDB or Virtuoso)
* Clone the project
```shell
git clone https://github.com/fair-workflows/openpredict.git
 ```
* Install docker to set up the environment

To install docker:  https://docs.docker.com/install/

* Build

From openpredict directory, edit workflow/config.yml file, set sparql_ep to the running SPARQL endpoint or your own SPARQL endpoint
```shell
cd openpredict/
docker build -t openpredict .
 ```
 
  * Run Juypter
```shell
docker run -d --rm --name openpredict -p 8888:8888 openpredict
 ```

* Execute CWL workflow

```shell
docker exec -it openpredict cwltool --outdir=/juypter/run/ workflow/openpredict-ipynb.cwl workflow/config.yml

--outdir	enter folder in which you want to generate the outputs
 
 ```
You would expect to see two juypter notebook output notebook files (output_fg.ipynb, output_ml.ipynb) and the other generated results to be stored in your outdir ('/juypter/run/')  
 
 



