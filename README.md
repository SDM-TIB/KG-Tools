# Scientific Data Management Lab 


# KG-Tools available @SDM

- [RDFizer](https://github.com/SDM-TIB/rdfizer) - Transforms raw data to RDF 
- [SemEP](https://github.com/SDM-TIB/SemEP-Node) - Community detection applied on knowledge graphs to find insights and patterns.
- [MULDER](https://github.com/SDM-TIB/MULDER) - Federated SPARQL Engine for distributed and autonomous SPARQL endpoints.
- [BOUNCER](https://github.com/SDM-TIB/BOUNCER) - Privacy-aware Query Processing Over Federations of RDF Datasets
- [Ontario](https://github.com/SDM-TIB/Ontario) - Federated SPARQL Engine for Heterogenous Data Sources in a Data Lake


# Running KG-Tools on Docker 


## Running RDFizer
```bash
$ docker run -d --name myrdfizer -p 4000:80 kemele/rdfizer:1.0
```
You propbably need to attach a volume to share data with the container, as follows:
```bash
$ docker run -d --name myrdfizer -p 4000:80 -v /path/to/data:/data kemele/rdfizer:1.0
```
Send a POST request with the configuration file to RDFizer the file:
```
$ curl -X POST localhost:4000/graph_creation/data/path/to/config-file
```
Config file should look something like this:
```
[default]
main_directory: /data
[datasets]
number_of_datasets: 1
output_folder: ${default:main_directory}/output

[dataset1]
name: ADSampleDataWP4CO
format: csv
path: ${default:main_directory}/rawdata/myfile.csv
mapping: ${default:main_directory}/mappings/mymap.ttl
remove_duplicate_triples_in_memory: yes
```

#### Example:
Raw data is stored in `/home/user/documents/mydata` and it contains three folders; `config`, `mappings` and `rawdata`. Our `config_file.ini` is in `config` folder, our RML mapping file, `mymap.ttl` is stored in `mappings` and our raw data `myfile.csv` is stored in `rawdata` folder. Run the following:
```
$ docker run -d --name myrdfizer -p 4000:80 -v /home/user/documents/mydata:/data kemele/rdfizer:1.0
```
Then execute the following to start the transformation of raw data:
```
$ curl -X POST localhost:4000/graph_creation/data/config/config_file.ini 
```
The output of the RDFization process will be written to a folder specified in the `output_folder` (in this case, it is in `/data/output`) as `N-Triple` file.
