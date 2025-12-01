# HCP
## Feature Diversity or Feature Interactions: Which Drive Effective Configuration Testing Prioritization for Highly Configurable Software?

### Overview
HCP is a tool designed for internal feature-interaction analysis and configuration prioritization in highly configurable software systems. It performs variability-aware static analysis to extract internal feature interactions and leverages them to guide configuration-level testing prioritization.

### How to Run HCP

## Subject Systems
In our study, we evaluate HCP using real faults collected by Medeiros et al. ([FCM](https://www.dsc.ufcg.edu.br/~spg/sampling/)) and the Variability Bugs Database ([VBDb](https://vbdb.bitbucket.io/)).

## Download and Install Joern
In our work, we rely on [Jess](https://github.com/LPhD/Jess.git), an extension of Joern, to parse variability-aware code property graphs (vaCPGs). You need to follow the Jess [documentation](https://jess.readthedocs.io/en/dev/) to complete installation and build.

## Parsing vaCPGs
After installing Jess, run the following command to parse the target system and import it into the graph database:

```sh
jess-import ./testProjects/systemPath databaseName
```

## Identifying Internal Feature Interactions
```sh
python ./customScripts/identify_global_internal_FIs.py -s databaseName -r ./testProjects/systemPath
```
The identification results are stored under `./testProjects/systemPath/`. The file `final_sampling.txt` records all potential feature interactions, and `PCwithStatements.csv` records the logical presence conditions of statements.

## Configuration Testing Prioritization
First, you need to sample configurations for the target system; you may choose any sampling algorithm you prefer. In our experiments, we use the integrated tool [AutoSMP](https://github.com/TUBS-ISF/AutoSMP.git), which provides several sampling algorithms, including Chvátal, ICPL, Yasa, and IncLing. During configuration testing prioritization, you need to provide the sampling directory via the `samplingPath` argument.

```sh
python ./customScripts/eval_apfd.py -system systemName -r ./testProjects/systemPath --spath samplingPath -c cnfPath -b bugfilepath -o outpath
```
