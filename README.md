# LPCA_mice_project

Exam date: 20-06-2025
Grade projet and presentation: 30L

 | Student            | ID       |
|--------------------|:--------:|
| Miriam Zara         | 2163328  |
| Giorgia Fasiolo     | 2159992  |
| Angela Bortolato    | 2156562  |
| Luca Volpi          | 2157843  |

## Overview
The advancement in DNA sequencing tecniques have enabled access to fine-detailed information about the composition of gut microbiota. There is a widespread interest in the scientific comunity around the goal of modeling and predicting the dynamics of these ecological communities, with the aim of producing innovative terapeutic interventions. However, althogh a wide variety of methods have been published in recent years, many of these are unprincipled and lack reliable validation methods. Some studies suggest that gut microbiomes cluster into distinct groups (enterotypes), while others argue that these patterns may arise as artifacts of statistical analysis rather than biological reality.

In our project, following the article "A macroecological description of alternative stable states reproduces intra- and inter-host variability of gut microbiome", Silvia Zaoli and Jacopo Grilli, we analyze experimental data of mice gut microbiota assuming a simple stochastic logistic model. This model, specifically, assumes that each bacterial species evolves independently from the others, thus it can be used as a simple, essential null-model to investigate if inter-species interactions can be at all inferred from the data.

The logistic model predicts that, after a transient, the species abundances will eventually settle down to a steady state, parametrized by two parametes: the carrying capacity, accounting for the global effect of the environment, and a parameter 
 quantifying the stochastic noise. We will follow the methods described in the article, which use a properly defined dissimilarity measure to assess stationarity, designed to overcome the scarsity of data samples.

This main task will require preliminary data processing and basic statistic analysis.

Optionally, in the case where the null model doesn't suffice to explain the data, we will try to infer the interaction matrix of species-species interaction assuming a simple stochastic Lockta Volterra model, and using linear regression with k-fold validation.

---

1. **Preprocessing and sample composition analysis**

    *  **Data preprocessing**: 

        group OTUs corresponding to same species, fix naming issues, double measures etc, retrieve total number of OTUs, total number of species, set up pandas DataFrames and OTU-species dictionary.

    *  **Analysis of the cross-subject sample composition variability at different taxonomic-unit levels:** 

        Biological organisms are classified in groups on the basis of similarity. Biologists refer to these groups as "taxonomic units" or taxa. Different taxa are also grouped together in a higher-level group ("taxon") based on commonly shared features, resulting in a nested hierarcical classification. The basic scheme of modern classification makes use of $8$ levels of classification, which are, from top to bottom: $\textit{Domain, Kindom, Phylum, Class, Order, Family, Genus, Species}$. 

        The task is to assess the cross-subject variability of the sample composition at the various levels of the taxonomic classification. The output will be given in the form of stacked-bar plots.


    *  **Rank Abundance Distribution (RAD):**
    
        The frequency of each species is computed (= #counts_species/ #counts sample) . Species are ranked with an integer index from the most frequent (i= 1) to the least frequent (i= N). The curve displaying the frequency versus the rank (so-called rank-abundance distribution, or RAD) is experimentally known to assume a lognormal or power-law -like shape, with the first few species occupying the majority of the sample, and a long tail of rare species contributing to the remaining part.


2. **Theoretical models**

    * **Logistic Model:**

        Describe Logistic Model in its Deterministic and Stochastic formulations. Describe the meaning of parameters (carry capacity, enviromental noise, time scale)
  
3. **Basic time-series analysis**

    Say $\vec{X}(t) =\left[x_1(t), x_2(t), \cdots x_N(t)\right]$ is the abudance of the first N most abudant species at time t. We model $\vec{X}(t)$ as a stochastic process, for which
    each subject $\vec{X}(t)$ represents a different sample path.

    * Visualization:
    
        Plot time-series for each mouse and for each species to visualize the evolution in time

    The basic analysis to perform is:
    
    * Compute the time dependent mean and variance $\mathbb{E}[\vec{X}(t)],\, \mathbb{E}[\vec{X}^2(t)]$. Plot as a summary the time evolution for the mean +- std of each species
    * Correlations: filter the most abundant species upon a certain threshold and compute their covariance matrix 
    * Autocorrelation: compute the autocorrelation function $\mathbb{E}[x(t) \cdot x(s)]$



4. **Stochastic Logistic Model**

    Here we follow the main reference *"A macroecological description of alternative stable states reproduces intra- and inter-host variability of gut microbiome", Silvia Zaoli and Jacopo Grilli*.

    * Dissimilarity:
    
        Calculate **dissimilarity** defined as $$\Phi_i(t,T)=\left(\frac{\lambda_i(t)-\lambda_i(t+T)}{\lambda_i(t)+\lambda_i(t+T)}\right)^2$$, where $\lambda_i(t)$ is the relative frequency of the species $i-th$ at time $t$, and $T$ is the log.
        Distinguish OTUs that have stationary dynamics from the others, visualize in a plot $\Phi_i(T)/\Phi_i^\infty$ vs T.  Discriminate between stationary and non-stationary abundancies (perform a linear fit, set a threshold and discriminate slopes above/below threshold).

    * Stationarity:

       - Discriminate between stationary and non-stationary abundancies (perform a linear fit, set a threshold and discriminate slopes above/below threshold)

        - Perform a taxonomic analysis on species with nonstationary behaviour (common classifications or something else)

    * Estimate the parameters of **Stochastic Logistic Model (SLM)** for stationary species:

        estimate the model parameters $K$ and $\sigma$, from the relations $\left<\lambda\right>=K\left(\frac{2-\sigma}{2}\right)$ and $Var(\lambda)= \left( \frac{\sigma}{2-\sigma} \right) \left<\lambda\right>^2$

        - if $\sigma<2$, the stationary distribution is gamma $$P(\lambda; K, \sigma) = \frac{1}{\Gamma (2/\sigma -1)} \left(\frac{2}{\sigma K}\right)^{2/\sigma -1} \lambda^{2/\sigma-2} e^{-\frac{2}{\sigma K}\lambda} $$
        so we can produce a histogram out of our data and superimpose the theoretical pdf; perform test on distribution

        calculate theoretical expectation value for the dissimilarity at stationarity $\mathbb{E}[\Phi_i^\infty]=\frac{\sigma}{4-\sigma}$, compare with the data.
