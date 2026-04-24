  ---

## 🎯 Research Questions

1. **Which demographic groups have higher readmission rates?**
   - Analysis across age, gender, and race

2. **What machine learning models best predict readmission risk?**
   - Comparison of Logistic Regression, Random Forest, and Neural Networks

3. **Which clinical features are most predictive of readmission?**
   - Feature importance analysis from linear models

4. **How does model performance vary across patient subgroups?**
   - Fairness evaluation across demographics

---

## 📈 Results Summary

### Model Performance

| Model | Validation AUC | Test AUC | 95% CI |
|-------|----------------|----------|---------|
| **Random Forest** | 0.6607 | **0.6685** | [0.6548, 0.6822] |
| Logistic Regression | 0.6401 | 0.6449 | [0.6310, 0.6588] |
| Neural Network | 0.6235 | 0.6237 | [0.6098, 0.6376] |

### Demographic Disparities

**Age:**
- Highest readmission: 20-30 years (14.1%)
- Lowest readmission: 0-10 years (1.8%)
- Pattern suggests young adults face unique diabetes management challenges

**Race:**
- Caucasian: 11.6%
- African American: 11.4%
- Hispanic: 10.5%
- Asian: 10.0%

**Gender:**
- Female: 11.1%
- Male: 10.8%

### Top Predictive Features

**Increasing Risk:**
1. Number of prior inpatient visits (coef: +0.30)
2. Discharge disposition (coef: +0.13)
3. Diabetes medication prescribed (coef: +0.08)
4. Number of diagnoses (comorbidity burden) (coef: +0.07)

**Decreasing Risk:**
1. Metformin prescription (coef: -0.06)
2. Number of procedures (coef: -0.05)
3. Insulin use (coef: -0.02)

---

## 🚀 Getting Started

### Prerequisites

- Python 3.8 or higher
- Jupyter Notebook or Google Colab
- 4GB+ RAM recommended

### Installation

**1. Clone the repository**
```bash
git clone https://github.com/megangadiparthy/diabetes-readmission-analysis.git
cd diabetes-readmission-analysis
```

**2. Install dependencies**
```bash
pip install -r requirements.txt
```

**3. Download the dataset**

The UCI Diabetes 130-US Hospitals dataset is not included in this repository due to size.

Download from: [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/diabetes+130-us+hospitals+for+years+1999-2008)

Or use wget:
```bash
mkdir data
cd data
wget https://archive.ics.uci.edu/ml/machine-learning-databases/00296/dataset_diabetes.zip
unzip dataset_diabetes.zip
cd ..
```

**4. Run the analysis**

**Option A: Google Colab (Recommended)**
1. Upload `Diabetes.ipynb` to Google Colab
2. Upload the dataset zip file when prompted
3. Run all cells

**Option B: Local Jupyter**
```bash
jupyter notebook Diabetes.ipynb
```

**Option C: Python Script**
```bash
python diabetes.py
```

---

## 📊 Dataset Information

**Source:** UCI Machine Learning Repository  
**Citation:** Strack, B., et al. (2014). "Impact of HbA1c measurement on hospital readmission rates: analysis of 70,000 clinical database patient records." *BioMed Research International*.

### Dataset Statistics

- **Hospital encounters:** 101,766
- **Unique patients:** 71,518
- **Hospitals:** 130 US institutions
- **Time period:** 1999-2008
- **Features:** 50 (demographic, clinical, medication)
- **Target variable:** Readmitted within 30 days (binary)

### Key Features

**Demographics:**
- Age, gender, race

**Clinical:**
- Number of diagnoses, procedures, medications
- Lab tests performed, HbA1c results
- Time in hospital, number of emergency visits

**Medications:**
- 23 diabetes medication features
- Medication changes during hospitalization

**Outcomes:**
- Discharge disposition
- Readmission status (<30 days, >30 days, no readmission)

---

## 🔬 Methodology

### 1. Data Preprocessing

- **Missing data:** Median imputation for continuous, mode for categorical
- **Categorical encoding:** Label encoding with one-hot for high-cardinality features
- **Feature engineering:** Interaction terms (medications × diagnoses, age × emergency visits)
- **Train/Val/Test split:** 60% / 20% / 20% stratified by outcome

### 2. Model Development

**Logistic Regression**
- Hyperparameters: C ∈ {0.001, 0.01, 0.1, 1, 10, 100}, L2 penalty
- 5-fold cross-validation

**Random Forest**
- Hyperparameters: n_estimators ∈ {100, 200}, max_depth ∈ {10, 20, 30}, min_samples_split ∈ {5, 10}
- 3-fold cross-validation (computational efficiency)

**Neural Network**
- Architectures: {(50,), (100,), (100, 50)} hidden layers
- Regularization: alpha ∈ {0.0001, 0.001, 0.01}
- Early stopping on validation set

### 3. Evaluation Metrics

- **Primary:** Area Under ROC Curve (AUC)
- **Confidence intervals:** 1000-iteration bootstrap
- **Subgroup fairness:** Accuracy, sensitivity, specificity by demographics

### 4. Feature Importance Analysis

- **Linear model coefficients** for interpretability
- Top 10 positive and negative features visualized

---

## 📊 Visualizations

All figures are generated automatically and saved to `outputs/`:

**Figure 1: Demographic Readmission Rates**
- Bar charts showing readmission rates by age, gender, and race
- Red dashed line indicates overall population rate

**Figure 2: Model Performance Comparison**
- Validation and test AUC for all three models
- Error bars show 95% confidence intervals from bootstrap

**Figure 3: Feature Importances**
- Horizontal bar charts of top 10 positive/negative features
- Based on logistic regression coefficients

**Figure 4: Subgroup Performance**
- 3×3 grid showing accuracy, sensitivity, specificity
- Across age groups, gender, and race
- Error bars show 95% confidence intervals

---

## 🔍 Key Insights & Clinical Implications

### 1. Model Performance
Random Forest achieved the best performance (AUC = 0.6685), exceeding the target range of 0.65-0.70. However, moderate discriminative ability suggests readmission is influenced by factors beyond clinical data, including social determinants of health.

### 2. Demographic Disparities
Significant variation in readmission rates across demographics highlights the need for targeted interventions:
- Young adults (20-30) may benefit from enhanced diabetes education and transition care programs
- Racial disparities suggest addressing social determinants (access, health literacy, cultural competence)

### 3. Clinical Risk Factors
- **Prior hospitalizations** are the strongest predictor, indicating care fragmentation
- **Discharge to post-acute facilities** signals inability to safely return home
- **Metformin** (first-line therapy) is protective, suggesting guideline-concordant care improves outcomes

### 4. Fairness Concerns
At high accuracy threshold (90%), the model shows:
- **Poor sensitivity** (<5%) overall—fails to identify most patients who will be readmitted
- **Disparate impact:** Elderly patients and certain racial groups have lower sensitivity
- **Clinical utility:** Current model better suited for ruling out readmission than identifying high-risk patients

### 5. Recommendations
- Lower threshold for clinical deployment to improve sensitivity
- Integrate social determinants of health data
- Implement fairness constraints in model training
- Use predictions to trigger care management interventions, not deny care

---

## 🛠️ Technologies Used

**Languages & Libraries:**
- Python 3.8+
- pandas (data manipulation)
- NumPy (numerical computing)
- scikit-learn (machine learning)
- matplotlib & seaborn (visualization)
- SciPy (statistical analysis)

**Development Environment:**
- Google Colab / Jupyter Notebook
- VS Code / GitHub Codespaces

**Version Control:**
- Git & GitHub

---

## 📝 Requirements

```txt
pandas>=1.5.0
numpy>=1.23.0
scikit-learn>=1.2.0
matplotlib>=3.6.0
seaborn>=0.12.0
scipy>=1.10.0
```

Install all at once:
```bash
pip install -r requirements.txt
```

---

## 🎓 Academic Context

This project was completed as Problem Set 1 for CPH 200C: Computational Precision Health Cornerstone at UC Berkeley. The assignment focused on:

1. **Risk stratification** from electronic health records
2. **Model development** across multiple algorithm classes
3. **Feature interpretation** for clinical insights
4. **Fairness evaluation** across patient subgroups

The analysis demonstrates practical application of machine learning to healthcare challenges while addressing ethical considerations around algorithmic fairness.

---

## 📚 References

**Primary Source:**
- Strack, B., DeShazo, J.P., Gennings, C., et al. (2014). "Impact of HbA1c measurement on hospital readmission rates: analysis of 70,000 clinical database patient records." *BioMed Research International*, 2014.

**Fairness in ML:**
- Kallus, N. & Zhou, A. (2019). "The fairness of risk scores beyond classification: Bipartite ranking and the xAUC metric." *NeurIPS*, 32.

**Confidence Intervals:**
- Cortes, C. & Mohri, M. (2004). "Confidence intervals for the area under the ROC curve." *NeurIPS*, 17.

**Clinical Context:**
- American Diabetes Association. (2024). "Standards of Medical Care in Diabetes."

---

## 🤝 Contributing

This is an academic project, but feedback and suggestions are welcome!

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit changes (`git commit -m 'Add improvement'`)
4. Push to branch (`git push origin feature/improvement`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 👤 Author

**Megan Gadiparthy**

- 🎓 UC Berkeley - Data Science & Molecular Cell Biology '27
- 💼 Founder & Lead Engineer, [Ascana](https://ascana.com) (Health Platform)
- 📱 iOS Developer, Berkeley Mobile Developers
- 🔗 [LinkedIn](https://linkedin.com/in/megan-gadiparthy)
- 🐙 [GitHub](https://github.com/megangadiparthy)
- 📧 megan.gadiparthy@berkeley.edu

---

## 🙏 Acknowledgments

- **UC Berkeley CPH 200C** teaching staff for assignment design
- **UCI Machine Learning Repository** for dataset access
- **Strack et al.** for original data collection and publication
- **scikit-learn community** for excellent ML tools

---

## 📞 Contact & Support

Questions about this project? 

- **Email:** megan.gadiparthy@berkeley.edu
- **GitHub Issues:** [Open an issue](https://github.com/megangadiparthy/diabetes-readmission-analysis/issues)

For dataset-related questions, consult the [UCI ML Repository](https://archive.ics.uci.edu/ml/datasets/diabetes+130-us+hospitals+for+years+1999-2008).

---

**⭐ If you found this project helpful, please consider starring the repository!**

---

*Last updated: April 2026*
