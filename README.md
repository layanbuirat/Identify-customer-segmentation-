# 📊 Identify Customer Segments - Project README

## Project Overview

This project was completed as part of the **Udacity Data Scientist Nanodegree Program**. The goal is to identify customer segments for a mail-order sales company in Germany using unsupervised learning techniques. By analyzing demographic data of the general population and comparing it with customer data, we can identify which population segments are most likely to become customers, enabling more targeted and efficient marketing campaigns.

---

## 📅 Project Timeline

| Start Date | End Date | Duration |
|------------|----------|----------|
| May 7, 2026 | May 15, 2026 | 8 days |

**Author:** Layan Buirat

---

## 🎯 Project Objectives

1. **Preprocess and clean** demographic data from the general population of Germany
2. **Identify patterns** in missing data and handle them appropriately
3. **Engineer features** from mixed-type and categorical variables
4. **Apply dimensionality reduction** using PCA to reduce noise and computational complexity
5. **Cluster the population** using K-Means to identify natural segments
6. **Compare customer data** to general population to identify overrepresented and underrepresented segments
7. **Provide actionable insights** for marketing campaigns

---

## 📁 Files and Data

### Input Files
- `Udacity_AZDIAS_Subset.csv` - Demographics data of Germany's general population (891,221 persons × 85 features)
- `Udacity_CUSTOMERS_Subset.csv` - Demographics data of the company's customers (191,652 persons × 85 features)
- `AZDIAS_Feature_Summary.csv` - Summary of feature attributes and types
- `Data_Dictionary.md` - Detailed descriptions of all features

### Output
- Cleaned and transformed datasets
- PCA-reduced feature space (49 components)
- Cluster assignments for both populations
- Comparative analysis of segment representation

---

## 🔧 Technical Approach

### Data Preprocessing

| Step | Description | Key Decisions |
|------|-------------|---------------|
| **Missing Value Handling** | Converted coded missing values (-1, 0, 9, 'X', 'XX') to NaN using `feat_info` dictionary | Used median imputation for numerical features |
| **Column Filtering** | Removed columns with >30% missing values | Removed 6 columns (TITEL_KZ, AGER_TYP, KK_KUNDENTYP, KBA05_BAUMAX, GEBURTSJAHR, ALTER_HH) |
| **Row Filtering** | Removed rows with >30% missing values (~10.5% of data) | High-missingness rows treated as separate segment in final analysis |
| **Categorical Encoding** | Kept binary categorical features, dropped multi-level ones | Used mapping for OST_WEST_KZ (W→0, O→1) |
| **Mixed-Type Features** | Engineered new features from complex columns | Created Youth_Decade & Youth_Movement from PRAEGENDE_JUGENDJAHRE; split CAMEO_INTL_2015 |

### Feature Engineering

**PRAEGENDE_JUGENDJAHRE (Youth Characteristics):**
- `Youth_Decade`: 1=1940s-50s, 2=1960s-70s, 3=1980s-90s
- `Youth_Movement`: 0=mainstream, 1=avantgarde

**CAMEO_INTL_2015 (Wealth & Life Stage):**
- `Cameo_Wealth`: Tens digit (1-5, wealth level)
- `Cameo_LifeStage`: Ones digit (1-5, life stage)

### Feature Transformation

| Technique | Parameters | Purpose |
|-----------|------------|---------|
| **SimpleImputer** | strategy='median' | Handle remaining missing values |
| **StandardScaler** | mean=0, std=1 | Normalize features for distance-based algorithms |
| **PCA** | 49 components (95% variance explained) | Dimensionality reduction and noise elimination |

### Clustering

| Parameter | Value | Justification |
|-----------|-------|----------------|
| **Algorithm** | K-Means | Effective for large datasets, interpretable results |
| **Number of clusters (k)** | 8 | Elbow method showed diminishing returns beyond k=8 |
| **Random state** | 42 | Reproducibility |
| **n_init** | 10 | Ensures convergence to good solution |

---

## 📈 Results and Insights

### Cluster Distribution Comparison

| Cluster | General Population | Customers | Difference | Ratio |
|---------|-------------------|-----------|------------|-------|
| 0 | 9.4% | 0.2% | -9.2% | 0.02 |
| 1 | 10.0% | 2.3% | -7.7% | 0.23 |
| 2 | 12.9% | 11.1% | -1.8% | 0.87 |
| 3 | 12.7% | 13.6% | +0.9% | 1.06 |
| 4 | 11.9% | 1.9% | -10.0% | 0.16 |
| 5 | 16.2% | 50.5% | +34.3% | 3.12 |
| 6 | 15.3% | 19.1% | +3.8% | 1.25 |
| 7 | 11.6% | 1.3% | -10.3% | 0.12 |

### Key Findings

#### 🎯 **Overrepresented Clusters (Target Audience)**

| Cluster | Overrepresentation | Key Characteristics |
|---------|-------------------|---------------------|
| **Cluster 5** | +34.3% (3.12x) | Environmentally conscious, financially prudent, mobile, urban |
| **Cluster 6** | +3.8% (1.25x) | Environmentally conscious, urban professionals, career-oriented |
| **Cluster 3** | +0.8% (1.06x) | Affluent, property-owning urban families |

**Persona Description - Cluster 5:**
- High `GREEN_AVANTGARDE` (environmental awareness)
- High `KBA05_ANTG1` (urban/rural indicator)
- High `FINANZ_MINIMALIST` (financial caution)
- High `SEMIO_VERT` (social responsibility)
- High `MOBI_REGIO` (regional mobility)

#### ⚠️ **Underrepresented Clusters (Not Interested)**

| Cluster | Underrepresentation | Key Characteristics |
|---------|--------------------|---------------------|
| **Cluster 7** | -10.3% | High-density urban dwellers, transient lifestyle |
| **Cluster 4** | -10.0% | Traditional conservatives, religious, dutiful |
| **Cluster 0** | -9.2% | Financially conservative, traditional values |
| **Cluster 1** | -7.7% | Socially oriented, culturally engaged |

---

## 💡 Business Recommendations

### Primary Marketing Strategy

**Target Clusters 5 and 6:**
- ✅ Focus on **digital marketing channels** (social media, email, mobile)
- ✅ Emphasize **sustainability and environmental consciousness**
- ✅ Highlight **value and financial responsibility**
- ✅ Target **urban professionals and renters**

### Secondary Marketing Strategy

**Target Cluster 3:**
- ✅ Focus on **premium product offerings**
- ✅ Use **family-oriented messaging**
- ✅ Consider **traditional media** (print, TV) for older demographics

### Segments to Avoid or Re-evaluate

| Cluster | Recommendation | Reason |
|---------|---------------|--------|
| **Cluster 7** | Avoid or use alternative channels | High transient population, difficult to reach |
| **Cluster 4** | Re-evaluate messaging | Traditional values conflict with product positioning |
| **Cluster 0** | Re-evaluate targeting | Financial conservatism may limit purchase behavior |

---

## 📊 PCA Component Interpretation

### Component 1: "Urban vs. Rural / Affluence"
- **Positive**: Urban density, wealth indicators, city size
- **Negative**: Regional mobility, low-density areas
- **Interpretation**: Captures the urban-rural divide combined with economic prosperity

### Component 2: "Financial Psychology & Generational Values"
- **Positive**: Older age, financial caution, achievement-oriented
- **Negative**: Traditional savers, religious values, dutiful behavior
- **Interpretation**: Captures generational and value-based differences in financial behavior

### Component 3: "Social Values & Gender"
- **Positive**: Social responsibility, cultural interest, family orientation
- **Negative**: Combative personality, male gender indicators, dominance
- **Interpretation**: Captures social values and interpersonal orientation

---

## 🛠️ Tools and Libraries Used

| Category | Tools |
|----------|-------|
| **Data Manipulation** | pandas, numpy |
| **Visualization** | matplotlib, seaborn |
| **Machine Learning** | scikit-learn (SimpleImputer, StandardScaler, PCA, KMeans) |
| **Statistical Testing** | scipy.stats (chi2_contingency) |
| **Environment** | Jupyter Notebook |

---

## ⚠️ Challenges and Solutions

| Challenge | Solution |
|-----------|----------|
| High missing value percentages in some columns (>30%) | Removed 6 outlier columns from analysis |
| String-based missing codes ('X', 'XX') | Modified conversion function to handle both numeric and string codes |
| Mixed-type features needing decomposition | Engineered separate features (Youth_Decade, Youth_Movement, Cameo_Wealth, Cameo_LifeStage) |
| High-missingness rows significantly different from main data | Treated as separate "High Missingness" segment in final comparison |
| Large dataset causing slow computation | Used PCA to reduce dimensions from 71 to 49 components |

---

## 📋 Key Code Functions

### Clean Data Function
```python
def clean_data(df, feat_info):
    """Complete preprocessing pipeline for demographics data"""
    # Convert missing codes to NaN
    # Remove columns/rows with >30% missing
    # Engineer mixed-type features
    # Encode categorical variables
    # Return numeric-only DataFrame
```

### PCA Weight Visualization
```python
def show_pca_weights(pca, component_idx, feature_names, top_n=10):
    """Display top features contributing to a principal component"""
```

---

## 📌 Conclusion

This project successfully identified distinct customer segments for the mail-order company. The key finding is that the company's strongest customer base consists of **urban, mobile, environmentally conscious, and financially prudent individuals** (Clusters 5 and 6), representing over 50% of all customers while only 16-15% of the general population.

The analysis also identified segments that are significantly underrepresented, suggesting that either:
1. The product/service does not appeal to these demographics
2. Alternative marketing channels are needed to reach them effectively

**Final Recommendation:** Focus marketing efforts on digital channels emphasizing sustainability and value, targeting urban professionals and renters, while re-evaluating strategies for traditional, conservative segments.

---

## 📚 References

- Udacity Data Scientist Nanodegree - Unsupervised Learning Module
- scikit-learn Documentation: PCA, KMeans, StandardScaler
- Bertelsmann Arvato Analytics - Data Dictionary

---

## 📧 Contact

**Author:** Layan Buirat  
**Course:** Udacity Data Scientist Nanodegree  
**Date:** May 7-15, 2026

---

*This project was completed as part of the Udacity Data Scientist Nanodegree program in collaboration with Bertelsmann Arvato Analytics.*
