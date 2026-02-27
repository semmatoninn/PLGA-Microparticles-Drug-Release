# Automated workflow in PLGA microparticles optimization using Active Learning

**Project Overview**

PLGA microparticles are widely used in drug delivery because they enable sustained release, which maintains therapeutic drug levels over extended periods while reducing dosing frequency and minimizing side effects. Sustained release is critical for improving patient adherence and ensuring stable pharmacokinetics in chronic treatments. The dataset, curated from an initial pool of 1,231 articles, comprises 321 in vitro PLGA microparticle release formulations spanning 89 unique drugs across 113 source publications by Zequing Bao.[1]

This project investigates how physicochemical properties of drugs and formulation parameters influence cumulative drug release from PLGA microparticles. The goal is to build predictive and interpretable machine learning models that can guide rational formulation design rather than relying on trial and error. [1][2][3]

**Step 1: Predicting Drug Release with XGBoost**

I first trained XGBoost models using molecular descriptors and formulation parameters to predict cumulative drug release profiles.
The objectives were to accurately predict release curves from descriptors and to understand which physicochemical properties play the most important role in PLGA microparticle drug delivery
The model successfully captured nonlinear interactions between drug properties and formulation parameters. This is important because drug release is rarely influenced by a single variable. Instead, it emerges from complex interactions among encapsulation efficiency, molecular weight, particle size, polymer degradation, and drug–polymer affinity.[2][3][4]
To interpret the model, I performed SHAP analysis within each cluster. The analysis revealed that these factors have the strongest influence on cumulative drug release.

* Encapsulation efficiency
* Drug molecular weight
* Particle size

When comparing predicted release curves to actual curves from the database, the model performed well for gradual release profiles but struggled with sharp burst release behavior. This suggests that burst release may involve additional mechanisms or structural factors not fully captured by the available descriptors.
This observation brings us back to the importance of physicochemical properties of drugs, formulation characteristics, their interactions, and the resulting mechanism of drug release.

**Step 2: Clustering Drugs in Chemical Space**

To better account for differences in release mechanisms, I applied UMAP to embed the descriptor space and clustered the drugs into four groups based on logP, TPSA, and molecular weight:

* Cluster 0: small hydrophobic molecules
* Cluster 1: large hydrophilic molecules
* Cluster 2: midsized amphiphilic drugs
* Cluster 3: hydrophilic drugs


Names were given to allow visualization of each cluster characteristics (drug size/lipopholiccity/polarity) from overarching properties of candidates drug per cluster [2][3][4][5]. More literatures are required to ensure accurate naming. 
According to the literature, these chemical groups tend to exhibit different release mechanisms, including:

* Gradual diffusion controlled release
* Sharp burst release
* Delayed release
* Degradation controlled release

**Step 3: Bayesian Optimization Objectives**

Instead of performing a global Bayesian Optimization that might overlook the specific chemical properties of each drug and its formulation, I performed Bayesian Optimization within each cluster.
This allows scientists who want to design drugs in a specific chemical space to use cluster-specific optimization recommendations as rational starting points. Based on sustained release criteria reported in the literature, I defined experimental goals for optimization:

* Burst control
* Minimum cummulative release threshold
* Monotonic release profile
* Sustained drug release tail beyond day 14
  
By optimizing within each chemical cluster toward these sustained release criteria, the workflow connects molecular properties, formulation parameters, machine learning prediction, and mechanism aware optimization into a coherent design strategy.

Dataset is by Zequing Bao from Dr. Allen lab

**References**

1. Bao, Z.; et al. Dataset of Drug Release from PLGA Microparticles. Sci. Data 2025, 12, Article 4621. https://doi.org/10.1038/s41597-025-04621-9.

2. Fredenberg, S.; Wahlgren, M.; Reslow, M.; Axelsson, A.The Mechanisms of Drug Release in Poly(lactic-co-glycolic acid)-Based Drug Delivery Systems—A Review. Int. J. Pharm. 2011, 415, 34–52. https://doi.org/10.1016/j.ijpharm.2011.05.049.

3. Park, K.; et al. Injectable Long-Acting PLGA Formulations: Development Challenges and Burst Release Issues. J. Controlled Release 2019, 304, 125–134. https://doi.org/10.1016/j.jconrel.2019.05.003.

4. Lim, M.; et al. Mechanisms of Drug Release in Long-Acting Injectable Microspheres and Design Parameters. Pharmaceutics 2022, 14, 614. https://doi.org/10.3390/pharmaceutics14030614.

5. Yoo, J.; Won, Y.-Y. Mechanistic Understanding of Initial Burst Release in PLGA Microparticles. ACS Biomater. Sci. Eng. 2020, 6, 6053–6065. https://doi.org/10.1021/acsbiomaterials.0c01228.
