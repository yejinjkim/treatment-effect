# Treatment Effect Estimation Pipeline in Electronic Health Records
Software use instruction for "Counterfactual Analysis of Comorbidity Risk Factors in Alzheimer’s Disease and Related Dementias" 

# Overview
Estimate treatment effect of common comorbidities in Alzheimer's disease

# Dependency
- `python` 3.6+
- `numpy`
- `scipy`
- `scikit-learn`
- `pandas`
- `networkx`
- `matplotlib`
- `doWhy` [0.5.1](https://github.com/microsoft/dowhy)
- `Tetrad ` [6.7.1](https://cloud.ccd.pitt.edu/nexus/content/repositories/releases/edu/cmu/tetrad-gui/)
# System Requirements
## Hardware Requirements
This pipeline only requires a standard computing resource to support the `Tetrad` and `doWhy`. 
Versions the software has been tested on: Memory 16GB, CPU Processor 4.2 GHz, 4 cores

## Software Requirements
The pipeline is tested on MacOS operating system (`Tetrad GUI 6.7.1`) and Linux (`doWhy`). 

### Install
Install the latest [release](https://pypi.org/project/dowhy/) using pip. 
``
pip install dowhy
``

Install the latest [release](https://pypi.org/project/dowhy/) using conda.
``
conda install -c conda-forge dowhy
``


# Demo
## Obtain Bayesian Network

Use `Tetrad` to derive Markov Blanket of outcome (e.g., Alzheimer's disease if the objective is to reduce Alzheimer's disease risk). 

>
> aaa
>






