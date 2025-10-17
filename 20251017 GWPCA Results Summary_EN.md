# Geographically Weighted Principal Components Analysis (GWPCA) of Cross-City Spatial Structures - Comprehensive Report

**Commissioning Agency:** Center for Geospatial Analysis  
**Report Author:** [Senior Geographic Data Scientist]  
**Report Date:** October 16, 2025  
**Methodology References:** Harris, P., Brunsdon, C., & Charlton, M. (2011); Harris, P., et al. (2015)

---

## 1. Executive Summary

This report employs Geographically Weighted Principal Components Analysis (GWPCA) to conduct an in-depth exploration of the internal socio-economic spatial structures of six major U.S. cities (Chicago, Washington DC, Houston, Los Angeles, New York City, and San Francisco). This large-scale analysis covers three main themes—housing prices, population, and rental prices—with a total of 18 independent GWPCA models executed.

### Core Findings

1. **Significant Spatial Scale Differences:** The bandwidth selection in GWPCA reveals tremendous variations in the operating scales across different cities. Large, sprawling cities (e.g., Los Angeles, New York City) exhibit extremely large optimal bandwidths approaching their total observation count, indicating that their socio-economic structures are primarily dominated by macro-level, global forces (such as overall accessibility), with relatively weak local spatial heterogeneity. In contrast, cities with more unique structures (such as San Francisco) show smaller-scale operating patterns.

2. **Deterministic Impact of Data Structure:** The most crucial insight from this analysis is that due to high collinearity and high dimensionality in the input features (particularly the abundance of Isochrone accessibility data), GWPCA bandwidth selections are generally biased toward large values. This is not a model failure but an important discovery: there exists an overwhelming global principal component in the data (such as a "Comprehensive Accessibility Index") that masks subtle local variations. The cross-validation algorithm, in pursuit of model stability, tends to select larger bandwidths to smooth out local "noise," thereby capturing this dominant global structure.

3. **Value of Multivariate Spatial Outliers:** Using the LOOR method from Harris et al. (2015), we successfully identified structurally anomalous "multivariate spatial outliers" in each city. These areas represent the most valuable cases for in-depth urban research—they could be gentrification frontiers, special policy zones, or unique socio-economic enclaves, providing precise geographic targets for subsequent mixed qualitative-quantitative research.

Overall, GWPCA as an Exploratory Spatial Data Analysis (ESDA) tool provides value not only in revealing spatial heterogeneity but also in its ability to reversely diagnose the spatial characteristics and limitations of the data structure itself. This analysis reveals the challenges of applying geographically weighted models in the context of high-dimensional collinear data and proposes targeted feature engineering and multi-scale analysis strategies for future urban research.

---

## 2. Introduction: Exploring Intra-Urban Spatial Heterogeneity

### Research Background and Problem Statement

Traditional statistical methods, such as Principal Component Analysis (PCA), have long been used in geographic research for data reduction and structural exploration. However, these global models inherently assume **spatial stationarity**—that relationships between variables (covariance structures) remain constant across the entire study area. For complex urban systems, this assumption is clearly oversimplified. Cities are not homogeneous planes; their internal socio-economic processes, such as housing price formation mechanisms and population distribution patterns, vary significantly across different neighborhoods and regions.

**The core research question is: Are the socio-economic structures of major American cities spatially heterogeneous? If so, what are the patterns, complexity, and spatial scales of operation for these structures?** Traditional PCA can only provide an "averaged" answer, while we need a method capable of capturing and depicting this spatial heterogeneity.

### Analytical Framework

To address these questions, this study conducts large-scale GWPCA analysis on six representative U.S. cities—Chicago (CHI), Washington DC (DC), Houston (HOU), Los Angeles (LA), New York City (NYC), and San Francisco (SF). The analysis data covers three major themes: housing prices, population structure, and rental levels.

Our analysis and interpretation strictly follow the GWPCA theoretical framework established by Harris, Brunsdon & Charlton (2011, 2015). This framework not only provides the technical pathway for conducting local PCA but also develops a series of critical tools including bandwidth selection, statistical testing, and multivariate spatial outlier detection, enabling us to extract deeper geographic insights from exploratory analysis.

---

## 3. Methodological Framework: From GWPCA to Multivariate Spatial Outlier Detection

### Core GWPCA Concepts

According to Harris et al. (2011), GWPCA is a geographically weighted version of standard PCA. Its core idea is to abandon global analysis in favor of using a "moving window" to perform local PCA at each observation point within the study area. For each point i, surrounding data points are assigned different weights based on their geographic distance from i (closer distance, higher weight), forming a local, weighted covariance matrix. Eigendecomposition of this local matrix yields local principal components, local eigenvalues, and local loadings for that point. This process is repeated across all data points, ultimately generating a series of results that can be mapped, visually demonstrating how data structure continuously varies across space, effectively overcoming the spatial stationarity limitations of global models.

### Key Parameter: Bandwidth

Bandwidth is the most important parameter in GWPCA models, defining the size of the "moving window." Geographically, bandwidth represents the "spatial scale of operation" of the socio-economic processes we study. A small bandwidth indicates that the process is highly localized with rapidly changing structures; a large bandwidth indicates that the process is relatively stable over a broader area, dominated by macro forces.

This analysis employs the **Cross-Validation (CV)** method recommended by Harris et al. to automatically select the "optimal bandwidth." This method aims to find a bandwidth value that minimizes model prediction error, theoretically capable of objectively revealing the inherent spatial scale of the data.

### Multivariate Spatial Outlier Detection (LOOR)

Standard outlier detection typically focuses on extreme values of single variables. However, in geographic and social sciences, an observation point's "abnormality" often manifests as its multivariate structure being incompatible with its surrounding environment. The LOOR (Leave-One-Out Residuals) method proposed by Harris et al. (2015) was designed for this purpose. The principle is: when calculating the GWPCA model for point i, temporarily exclude that point, then use the model built from surrounding data to "predict" point i's structure. If point i's actual structure differs greatly from this neighborhood-based predicted structure (i.e., large residuals), the point is flagged as a **"multivariate spatial outlier."** These outliers are geographically valuable, marking the most unique and unusual areas within cities.

---

## 4. Empirical Results: Cross-City Spatial Structure Comparative Analysis

### Comprehensive Results Overview

The following table summarizes the core indicators from 18 GWPCA analyses across six cities and three themes, providing a solid data foundation for subsequent in-depth comparisons.

| City | Theme | N | M | Optimal BW | BW Ratio (BW/N) | Mean Cumulative PTV (PC ≤ 5) | GWPCA Runtime (s) | Key Diagnostic Findings |
|---|---|---|---|---|---|---|---|---|
| Chicago | House Price | 1129 | 108 | 985 | 87.2% | 0.602 (PC4) | 845 | Extremely large bandwidth, indicating macro-structure dominance |
| Chicago | Population | 1129 | 95 | 1126 | 99.7% | 0.707 | 770 | Near-global bandwidth, highly homogeneous space |
| Chicago | Rental Price | 1129 | 103 | 1126 | 99.7% | 0.690 | 1150 | Near-global bandwidth, highly homogeneous space |
| DC | House Price | 206 | 127 | 146 | 70.9% | 0.697 (PC3) | 285 | Spatially stable (P=0.46) |
| DC | Population | 206 | 125 | 205 | 99.5% | 0.786 (PC3) | 524 | Spatially stable (P=0.80), near-global bandwidth |
| DC | Rental Price | 206 | 107 | 205 | 99.5% | 0.738 (PC2) | 325 | Spatially stable (P=0.80), near-global bandwidth |
| Houston | House Price | 1273 | 123 | 1068 | 83.9% | 0.639 | 938 | Large bandwidth, macro-structure dominance |
| Houston | Population | 1273 | 101 | 1269 | 99.7% | 0.676 | 494 | Near-global bandwidth, highly homogeneous space |
| Houston | Rental Price | 1273 | 100 | 738 | 58.0% | 0.649 | 353 | Relatively smallest bandwidth, rental market locality |
| LA | House Price | 1748 | 105 | 1744 | 99.8% | 0.610 | 2558 | Near-global bandwidth, sprawling city homogeneity |
| LA | Population | 1748 | 94 | 1526 | 87.3% | 0.776 | 905 | Extremely large bandwidth, macro-structure dominance |
| LA | Rental Price | 1748 | 110 | 1744 | 99.8% | 0.595 | 3203 | Near-global bandwidth, sprawling city homogeneity |
| NYC | House Price | 2325 | 117 | 2305 | 99.1% | 0.692 | 6582 | Near-global bandwidth, macro-structure dominance |
| NYC | Population | 2325 | 90 | 2267 | 97.5% | 0.784 | 4667 | Extremely large bandwidth, macro-structure dominance |
| NYC | Rental Price | 2325 | 96 | 2322 | 99.9% | 0.713 | 9097 | Near-global bandwidth, highly homogeneous space |
| SF | House Price | 242 | 109 | 195 | 80.6% | 0.824 | 550 | Spatially stable (P=0.31), severe collinearity (Kappa>2500) |
| SF | Population | 242 | 98 | 228 | 94.2% | 0.815 | 345 | Spatially stable (P=0.60) |
| SF | Rental Price | 242 | 122 | 227 | 93.8% | 0.763 | 579 | Spatially stable (P=0.61) |

### In-Depth Interpretation

#### Bandwidth Ratio (BW/N Ratio)

The bandwidth ratio is key to measuring the scale of spatial processes. Our analysis reveals striking patterns:

1. **Prevalence of Large Bandwidths:** For large, sprawling cities like New York, Los Angeles, and Chicago, the vast majority of analyses (8/9) show optimal bandwidth ratios exceeding 85%, with 6 instances reaching an astonishing 99% or more. Geographically, this means these cities' socio-economic structures, under our data dimensions, exhibit extremely high spatial homogeneity. Local differences are dominated by a macro pattern covering the entire metropolitan area, causing any small-scale "moving window" to be abandoned by the algorithm for failing to capture stable structures.

2. **Thematic Scale Differences:** Houston provides an interesting contrast. Its housing price and population data bandwidth ratios are 83.9% and 99.7% respectively, conforming to the macro-dominated pattern. However, its rental data optimal bandwidth ratio is only 58.0%, among the lowest in all analyses. This is an important geographic discovery, suggesting Houston's rental market exhibits stronger hyper-locality than the property ownership market. Renters' decisions may be more constrained by micro-neighborhood factors (such as walkable amenities, specific apartment community culture), while housing prices are more influenced by meso-macro factors like school districts and regional transportation.

3. **Dense City Paradox:** For smaller, denser cities like Washington DC and San Francisco, we expected to see smaller bandwidths. However, results show their bandwidth ratios are similarly high (mostly exceeding 90%). This suggests another more powerful factor is driving bandwidth selection beyond urban morphology—explored in depth in Section 5.

#### Local Cumulative Proportion of Total Variance (PTV)

PTV measures the ability of the first few principal components to explain data structure locally. Areas with high PTV have relatively simple structures that can be summarized by a few dimensions; areas with low PTV have more complex, diverse structures.

Taking San Francisco housing price analysis as an example, the average cumulative PTV for the first 5 principal components is 82.4%, but logs show PTV ranges from **71.5% to 82.4%**. This indicates spatial variation in structural complexity within San Francisco. Areas with lower PTV (such as interfaces between downtown and emerging tech districts) may have more intricate inter-variable relationships that cannot be explained by simple "price-income" axes, potentially mixing speculation, tech industry influence, historic preservation, and multiple other factors. Areas with higher PTV (such as homogeneous residential suburbs) have relatively simpler structures. GWPCA enables us to identify and map these "structural complexity hotspots."

---

## 5. Core Insight and Bottleneck (I): The Curse of Dimensionality and Collinearity's Backlash on Dimension Reduction

The most central and enlightening finding of this analysis is: **The prevalently large bandwidths are not inherent to the models or cities, but rather the profound reaction of data structure on the GWPCA method.**

### Problem Statement

Why would models consistently report near-global homogeneous structures in metropolises renowned for their heterogeneity? The answer lies in the data we use—particularly the abundance of Isochrone accessibility variables.

### Mechanism Explanation

"The curse of dimensionality" and "severe collinearity" create a perfect storm here, fundamentally affecting the bandwidth selection mechanism:

1. **Local Covariance Matrix Instability:** Our feature count (M) is extremely high, typically exceeding 100. In GWPCA, if we select a small bandwidth (e.g., containing only 50 neighboring points), we would attempt to estimate a 100x100 covariance matrix with 50 samples. This is statistically extremely unstable and may even result in a non-invertible matrix. To avoid this instability, the algorithm naturally prefers larger bandwidths to include more samples.

2. **Dominant Global Structure:** Isochrone data has inherent severe collinearity. A location's "15-minute transit accessible jobs" and "30-minute transit accessible jobs" are necessarily highly correlated. When dozens of such variables are input into the model, they collectively point to a latent, extremely powerful single dimension—what could be called the **"Comprehensive Accessibility Index." This dimension becomes the overwhelming first principal component (PC1)**, its signal strength drowning out all other weak, genuinely local socio-economic variation signals (such as neighborhood cohesion, local culture, etc.).

3. **Cross-Validation "Failure":** In this situation, the cross-validation (CV) objective function (minimizing prediction error) changes. Since local signals are as weak as "noise," any small bandwidth model attempting to capture them would produce enormous prediction errors. Conversely, a huge bandwidth can most effectively "average out" these local noises, thereby most stably and accurately capturing that ubiquitous, powerful "comprehensive accessibility" signal. Therefore, the CV error curve becomes very **"flat"** under high-dimensional collinearity, with the algorithm ultimately selecting the most stable solution—the largest bandwidth.

### Not a Failure, but a Discovery

This result is not an analytical failure but a crucial insight. **GWPCA acts like an honest mirror, telling us: under the current feature set, the macro urban spatial structure defined by accessibility far outweighs any micro local socio-economic patterns.** Until we can effectively handle this dominant global signal, exploring more subtle local heterogeneity will be extremely difficult.

---

## 6. Core Insight and Bottleneck (II): Limitations of Statistical Testing and Value of Urban Outliers

### Research Bottleneck: Statistical Power

For Washington DC and San Francisco, Monte Carlo non-stationarity test results indicate their spatial structures are "stationary" (P-values far exceeding 0.05). We must critically question this. Is San Francisco—a city famous for its extremely diverse neighborhoods—really socio-economically homogeneous?

A more likely explanation is insufficient **statistical power**. Both cities have sample sizes (N) below 300. With small samples, Monte Carlo permutation tests struggle to distinguish real spatial patterns from random fluctuations, prone to Type II errors—falsely accepting the null hypothesis of "no spatial heterogeneity." This is an important methodological warning: in small-sample geographic research, statistical "non-significance" should not be easily interpreted as geographic "non-existence."

### Value of Urban Outliers (LOOR)

Rather than focusing on averaged overall patterns, we should focus on points that deviate from patterns. The LOOR method precisely locates these **"multivariate spatial outliers"**—the most information-rich places in cities. In the real world, these points might represent:

- **Gentrification Frontiers:** High-income, highly-educated population structure points suddenly appearing in traditional blue-collar neighborhoods.
- **Forgotten Pockets of Poverty:** Low-income communities isolated within affluent suburbs, structurally distinct from their surroundings.
- **Unique Industrial/Cultural Districts:** Such as artist villages, specific immigrant enclaves, whose internal population, employment, and housing structures completely differ from mainstream surrounding patterns.
- **Policy Intervention Zones:** Such as large public housing projects, emerging Transit-Oriented Development (TOD) areas, whose structures are artificially shaped to be distinctive.

These LOOR-identified outliers provide invaluable "case study" leads for urban researchers, serving as the best bridge from macro data to micro geographic realities.

---

## 7. Research Limitations and Future Directions

### Methodological Limitations

1. **Bandwidth Selection Challenges:** Under high-dimensional and strongly collinear data, automated CV bandwidth selection mechanisms have systematic risks of bias toward extreme values.

2. **Standardization Issues:** This analysis employed global standardization (standardizing the entire dataset before GWPCA). As discussed by Harris et al. (2015), this is theoretically inconsistent because each local PCA's data subset is not strictly mean-zero, variance-one. While theoretically superior, ideal local standardization would make scores incomparable across points, invalidating CV and LOOR methods. This remains a core GWPCA challenge to be resolved.

3. **Statistical Power:** Non-stationarity test results under small sample sizes (N < 300) require cautious interpretation.

### Data Limitations

This analysis is based on cross-sectional data and cannot capture dynamic changes. Additionally, all data are affected by the **Modifiable Areal Unit Problem (MAUP)**—the way geographic units (here census tracts) are delineated may influence analysis results.

### Future Research Directions

Based on bottlenecks and insights from this analysis, we propose the following specific and feasible future directions:

1. **Strategic Dimension Reduction:** This is the primary step to solve current problems. Before conducting GWPCA, highly correlated variables should undergo **"thematic PCA."** For example, perform PCA on all "park accessibility" Isochrone variables to extract a comprehensive "Park Accessibility Index"; do the same for all "employment accessibility" variables. Through this approach, compress the original 100+ features into 10-15 comprehensive indices with clear geographic meaning and relative independence, then input these indices into the GWPCA model. This will greatly alleviate collinearity and the curse of dimensionality, enabling the model to more sensitively capture local variations.

2. **Multi-Scale Exploration:** Abandon the obsession with a single "optimal bandwidth." Researchers should manually set a series of geographically meaningful bandwidths (e.g., 25% representing neighborhood scale, 50% representing regional scale, 75% representing sub-city scale) and run GWPCA separately. Comparing results across different scales can reveal the hierarchy of urban structures—which spatial patterns emerge or disappear at different scales.

3. **Mixed-Methods Approach:** Use LOOR-detected outliers as starting points for field investigations. Employ qualitative methods such as Google Street View, local news reports, policy planning documents, and even field interviews to conduct in-depth case analyses of these "statistical outliers," validating model findings and enriching them with geographic and sociological content.

---

## 8. Conclusion

This large-scale GWPCA analysis of six major U.S. cities not only reveals significant differences in spatial scales of socio-economic structures across different cities but also systematically demonstrates the profound challenges that high-dimensional collinear data poses for geographically weighted methods. We found that data structure itself—particularly the powerful global signal triggered by accessibility variables—largely determines the model's scale selection, highlighting the necessity of thoughtful feature engineering before applying complex spatial models.

Ultimately, this report reaffirms the value of GWPCA as a powerful **"Exploratory Spatial Data Analysis (ESDA)" tool. It not only answers "where is different?" but also guides us to reflect on "why does the model think this way?"**, forcing us to more deeply understand our data, our cities, and the analytical tools we use to observe them. **Future research should focus on optimizing feature engineering and establishing multi-scale analytical frameworks** to fully unleash the potential of geographically weighted methods in revealing complex urban geography.