# Automated workflow in PLGA microparticles optimization using Active Learning

**Project Overview**
PLGA microparticles are widely used in drug delivery because they enable sustained release, which maintains therapeutic drug levels over extended periods while reducing dosing frequency and minimizing side effects. Sustained release is critical for improving patient adherence and ensuring stable pharmacokinetics in chronic treatments.
This project investigates how physicochemical properties of drugs and formulation parameters influence cumulative drug release from PLGA microparticles. The goal is to build predictive and interpretable machine learning models that can guide rational formulation design rather than relying on trial and error.
**Step 1: Predicting Drug Release with XGBoost**
I first trained XGBoost models using molecular descriptors and formulation parameters to predict cumulative drug release profiles.
The objectives were:
Accurately predict release curves from descriptors
Understand which physicochemical properties play the most important role in PLGA microparticle drug delivery
The model successfully captured nonlinear interactions between molecular properties and formulation parameters. This is important because drug release is rarely governed by a single variable. Instead, it emerges from complex interactions among encapsulation efficiency, molecular weight, particle size, polymer degradation, and drug–polymer affinity.
To interpret the model, I performed SHAP analysis within each cluster. The analysis revealed that:
Encapsulation efficiency
Drug molecular weight
Particle size
have the strongest influence on cumulative drug release.
When comparing predicted release curves to actual curves from the database, the model performed well for gradual release profiles but struggled with sharp burst release behavior. This suggests that burst release may involve additional mechanisms or structural factors not fully captured by the available descriptors.
This observation brings us back to the importance of physicochemical properties of drugs, formulation characteristics, their interactions, and the resulting mechanism of drug release.
**Step 2: Clustering Drugs in Chemical Space**
To better account for differences in release mechanisms, I applied UMAP to embed the descriptor space and clustered the drugs into four groups based on logP, TPSA, and molecular weight:
Cluster 0: small hydrophobic molecules
Cluster 1: large hydrophilic molecules
Cluster 2: midsized amphiphilic drugs
Cluster 3: hydrophilic drugs
According to the literature, these chemical groups tend to exhibit different release mechanisms, including:
Gradual diffusion controlled release
Sharp burst release
Delayed release
Degradation controlled release

**Step 3: Bayesian Optimization Objectives**
Instead of performing a global Bayesian Optimization that might overlook the specific chemical properties of each drug and its formulation, I performed Bayesian Optimization within each cluster.
This allows scientists who want to design drugs in a specific chemical space to use cluster-specific optimization recommendations as rational starting points. Based on sustained release criteria reported in the literature, I defined experimental goals for optimization:
Burst control
Minimum total release threshold
Monotonic release profile
Sustained tail beyond day 14
By optimizing within each chemical cluster toward these sustained release criteria, the workflow connects molecular properties, formulation parameters, machine learning prediction, and mechanism aware optimization into a coherent design strategy.
