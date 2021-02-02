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
```
pip install dowhy
```

Install the latest [release](https://pypi.org/project/dowhy/) using conda.
```
conda install -c conda-forge dowhy
```


# Demo

## Obtain Bayesian Network

Use `Tetrad` to derive Markov Blanket of outcome (e.g., Alzheimer's disease if the objective is to reduce Alzheimer's disease risk). Detailed tutorial is available at [here](https://rawgit.com/cmu-phil/tetrad/development/tetrad-gui/src/main/resources/resources/javahelp/manual/tetrad_tutorial.html).

1. Load csv data (Data->File->Validate->Load). See [sample](https://github.com/yejinjkim/treatment-effect/blob/main/occurrence_sample.txt).
```
dx1 dx2 dx3 ... |AD
-------------------
0.1 0.0 0.9     | 1
0.1 0.0 0.9     | 1
0.5 3.0 5.1     | 0
0.3 0.6 3.5     | 0

```
2. Add knowledge (e.g., tiers on temporality, black/white list)
3. Learn search (We used Markov Blanket + PC method implemented in MBFS)
4. Export the Bayesian Network via txt file via Graph option. See [sample](https://github.com/yejinjkim/treatment-effect/blob/main/bayesian-network-sample.txt).
```
dx1 -> dx2
dx2 -> dxn
dx3 -> AD
...
```

## Build causal model
Load Bayesian network and data. `causal_graph` is the Bayesian network derived from Tetrad. Details are at [sample notebook](https://github.com/yejinjkim/treatment-effect/blob/main/sample-code.ipynb).
```
import dowhy
from dowhy import CausalModel
import networkx as nx

data = pd.read_csv(data_path+data_file)
treatment='v_dx1'
outcome='v_AD'

model=CausalModel(data=data,
                 treatment=treatment,
                 outcome=outcome,
                 graph=data_path+causal_graph+'.gml')
              
```

## Estimate treatment effect
Identify estimand
```
identified_estimand=model.identify_effect(proceed_when_unidentifiable=True)
```


Estimate average treatment effect (ATE) and ATE among treated (ATT)
```
 estimate=model.estimate_effect(identified_estimand,
                               method_name='backdoor.propensity_score_weighting',
                                method_params={"weighting_scheme":"ips_normalized_weight"},
                               target_units='att')
```

Hypothesis test on Fisher's Null. Test whether the ATE is random or not
```
res_random=model.refute_estimate(identified_estimand, estimate, method_name="random_common_cause")
```



