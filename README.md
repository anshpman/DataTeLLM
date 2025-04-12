# DataTeLLM: Automated Dataset Storyteller with LLM Insights


DataTeLLM is a Python-based tool designed to accelerate the initial Exploratory Data Analysis (EDA) process for tabular datasets. It automates common analysis tasks and uniquely leverages a Large Language Model (Google Gemini) to generate narrative descriptions, interpret findings, identify potential risks, and suggest next steps.

**Problem:** Manually performing EDA, generating plots, calculating stats, identifying issues, and summarizing insights for every new dataset is repetitive and time-consuming.

**Solution:** DataTeLLM provides a "story" of the dataset quickly by creating a comprehensive HTML and PDF report, enabling faster understanding and decision-making.
# Key Features

* **Automated EDA:** Performs core analysis including:
    * Dataset shape, head, tail, data types.
    * Descriptive statistics for all columns.
    * Missing value identification and quantification.
    * Cardinality analysis (unique values).
* **Visualizations:** Generates and embeds (Base64):
    * Histograms for numerical features
    * Count plots for categorical features
    * Correlation matrix heatmap
    * PCA scatter plot for clustering results
* **Advanced Analysis:** Includes:
    * Correlation analysis with identification of highly correlated pairs
    * Anomaly detection using Isolation Forest
    * Clustering using K-Means (K=4 default)
    * Time Series applicability check
* **LLM Integration (Google Gemini):**
    * Generates narratives explaining analysis results
    * Provides suggestions/implications based on findings (missing data, skew, correlation, clusters)
    * Synthesizes a data quality risk summary
    * Offers speculative domain insights based on column names/types
* **Reporting:** Outputs comprehensive reports in both:
    * HTML format.
    * PDF format (requires WeasyPrint).
## Technology Stack

* **Core:** Python 3.10+, Pandas, NumPy
* **Visualization:** Matplotlib, Seaborn
* **Analysis:** Scikit-learn (StandardScaler, SimpleImputer, KMeans, IsolationForest, PCA), Statsmodels
* **LLM:** google-generativeai (Google Gemini API)
* **Reporting:** WeasyPrint (for PDF), IPython (for notebook display)
* **Environment:** Developed in Kaggle Notebooks
## How It Works: The DataTeLLM Pipeline

DataTeLLM follows a structured pipeline to analyze the input dataset and generate the final report:

1.  **Setup & Initialization:**
    * Loads the input dataset using Pandas.
    * Initializes a Python dictionary (`analysis_results`) to store all findings, plot paths, and narratives.
    * Configures the Google Gemini API client using the provided API key.

2.  **Sequential Analysis Modules:** The script calls a series of dedicated Python functions, each performing a specific analysis task:
    * `run_core_analysis`: Calculates basic info, descriptive statistics, missing values, unique counts, and stores head/tail.
    * `run_visualizations`: Identifies numerical/categorical columns, generates histograms and count plots (saving as `.png`), calculates skewness. Stores plot paths and skewness values.
    * `run_correlation`: Calculates the correlation matrix for numerical features, generates and saves a heatmap, identifies and stores significant pairs.
    * `run_anomaly_detection`: Preprocesses selected numerical features (imputation), applies `IsolationForest` to detect outliers, and stores the count/percentage of anomalies.
    * `run_clustering`: Preprocesses selected numerical features (imputation, scaling), performs K-Means clustering (K=4 default), calculates cluster characteristics (mean feature values), generates and saves a PCA visualization of clusters.
    * `find_time_column` & `run_time_analysis`: Checks for a potential time/date column and, if found, attempts basic trend plotting and seasonal decomposition.

3.  **LLM Narrative & Suggestion Generation (`run_llm_generation`):**
    * After all analyses are complete, this function gathers the results stored in the `analysis_results` dictionary.
    * It constructs specific prompts for the Google Gemini API based on these results (e.g., providing summary stats, skewness values, correlation pairs, cluster means, missing data info, anomaly counts).
    * It makes multiple calls to the LLM to generate:
        * Descriptive narratives for each analysis section.
        * Potential implications or suggestions based on key findings.
        * A synthesized Risk Identification summary.
        * Speculative Domain Insights.
    * All generated text is stored back into the `analysis_results` dictionary.

4.  **Report Generation (`generate_html_report` & `create_pdf_from_html`):**
    * The `generate_html_report` function takes the final `analysis_results` dictionary.
    * It dynamically builds an HTML document, structuring the content according to predefined sections.
    * It embeds tables (using `df.to_html`), plots (using Base64 encoding), and all LLM-generated narratives/suggestions.
    * If WeasyPrint is installed, the `create_pdf_from_html` function takes the generated HTML content and converts it into the final PDF report.

This sequential process, combined with LLM integration at the interpretation stage, allows DataTeLLM to produce a comprehensive and insightful initial analysis report automatically.
## Example of Titanic Dataset

 This dataset contains information on 418 passengers, including their survival status
 (Survived), passenger class (Pclass), sex (Sex), age (Age), and other details.
 Analysis of this data could reveal correlations between survival and factors like
 passenger class, age, and gender. Understanding these relationships may provide
 insights into the dynamics of survival during the Titanic disaster
 
## Example of Student Mental Health Dataset

This dataset contains information on 27,901 individuals, encompassing their gender, age, and various factors related to mental health. The data includes details on academic and work pressure, sleep duration, dietary habits, and family history of mental illness, among other variables. Analysis of this data could reveal correlations between these factors and the prevalence of depression.

**Section: Overview (showing selected columns and note)**
*[Insert Screenshot of Head/Tail table like image_7a5e52.png]*

**Section: Clustering Visualization**
*[Insert Screenshot of the PCA Scatter Plot like the one in image_85bc17.png]*

**Section: Risk Identification Summary**
*[Insert Screenshot of the Risk ID text from the report]*

**Section: LLM Suggestions (Example from Correlation)**
*[Insert Screenshot of one of the suggestion boxes]*

*[Optional: Link to a full sample analysis_report_final.pdf file]*
