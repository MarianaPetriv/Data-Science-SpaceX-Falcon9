# SpaceX Falcon 9 First Stage Landing Prediction

## Table of Contents
1. [Introduction](#1-introduction)
   - [Project Background](#11-project-background)
   - [Problem Statement](#12-problem-statement)
   - [Project Objectives](#13-project-objectives)
2. [Data Collection](#2-data-collection)
   - [SpaceX API](#21-spacex-api)
   - [Web Scraping](#22-web-scraping)
3. [Data Wrangling](#3-data-wrangling)
4. [Exploratory Data Analysis](#4-exploratory-data-analysis)
   - [SQL Queries](#41-sql-queries)
   - [Data Visualization](#42-data-visualization)
5. [Predictive Analysis](#5-predictive-analysis)
6. [Conclusion](#6-conclusion)

## 1. Introduction

### 1.1. Project Background
SpaceX revolutionized the aerospace industry with the Falcon 9's cost-efficiency, driven by its first stage's reusability. 

The Falcon 9's first stage can land back on Earth after launch, ready to be reused for another mission. This capability significantly lowers the cost of space travel, becoming an industry game-changer.

### 1.2. Problem Statement
The goal of this project is to use machine learning models to predict whether the first stage of a Falcon 9 rocket will successfully land, based on data collected from previous launches.

### 1.3. Project Objectives
- **Exploratory Data Analysis (EDA):** Analyze and understand the dataset to identify patterns, trends, and key features related to the landing outcome.
- **Data Preparation:** Preprocess the data, standardize it, and split it into training and testing sets.
- **Model Building:** Train and evaluate multiple machine learning models, including Logistic Regression, Support Vector Machines, Decision Trees, and K-Nearest Neighbors.
- **Model Evaluation:** Assess the models' performance, choosing the best accuracy.
- **Drawing Conclusions:** Summarize the key findings and select the best-performing model.

## 2. Data Collection

### 2.1. SpaceX API
**Objectives:**
- Request data from the SpaceX API.
- Clean the requested data.

We are specifically interested in the following data:
- **Rocket**: Learn the booster name.
- **Payload**: Learn the mass and orbit of the payload.
- **Launchpad**: Learn the name, longitude, and latitude of the launch site.
- **Cores**: Learn the landing outcome, type of landing, number of flights with that core, usage of gridfins, core reuse, use of legs, landing pad, core block version, number of times the core has been reused, and core serial number.

Four functions were provided by IBM instructors to save time when using the API to gather the data:
- `getBoosterVersion()`: Uses the rocket column to call the API and append data to the list.
- `getLaunchSite()`: Uses the launchpad column to call the API and append data to the list.
- `getPayloadData()`: Uses the payloads column to call the API and append data to the lists.
- `getCoreData()`: Uses the cores column to call the API and append data to the lists.

### 2.2. Web Scraping
**Objectives:**
- Web scrape Falcon 9 launch records using BeautifulSoup.
- Extract the Falcon 9 launch records from a Wikipedia HTML table.
- Parse the table and convert it into a Pandas DataFrame.

## 3. Data Wrangling
**Objectives:**
- Convert the data into a more usable form.
- Determine Training Labels:
  - `1` means the booster successfully landed.
  - `0` means the booster did not land successfully.

**Feature Engineering:**
- **True Ocean**: Mission outcome successfully landed in a specific ocean region.
- **False Ocean**: Mission outcome unsuccessfully landed in a specific ocean region.
- **True RTLS**: Mission outcome successfully landed on a ground pad.
- **False RTLS**: Mission outcome unsuccessfully landed on a ground pad.
- **True ASDS**: Mission outcome successfully landed on a drone ship.
- **False ASDS**: Mission outcome unsuccessfully landed on a drone ship.
- **None ASDS**: Represents a failure to land on a drone ship.

## 4. Exploratory Data Analysis

👉 We will now perform some Exploratory Data Analysis (EDA) to find some patterns in the data.

### 4.1. SQL Queries
**Objectives:**
- Understand the SpaceX dataset.
- Load the dataset into the corresponding table in a Db2 database.
- Execute SQL queries to answer assignment questions.

### 4.2. Data Visualization
**Objectives:**
- Perform exploratory data analysis and feature engineering using Pandas and Matplotlib.

**Findings:**
- **CCAFS LC-40:** No strong relationship between flight number and success rate. Success appears consistent across various flight numbers.
- **KSC LC-39A:** Similarly, no strong correlation between flight number and success rate. Success rates vary across flight numbers.
- **VAFB SLC 4E:** Success rates appear consistent, regardless of flight number.

**Findings (Payload vs Launch Sites):**
- **VAFB-SLC:** No rockets launched for heavy payload mass (greater than 10,000).
- **Other Launch Sites:** Rockets launched with a range of payload masses.
- **The launch site affects payload capacity and handling.**

**Findings (Orbit Types):**
- **SSE, HEO, GEO, ES-L1 Orbits:** 100% success rates, indicating high reliability.
- **Other Orbit Types:** Varying success rates, suggesting mission complexities.

**Findings (Payload vs Orbit Type):**
- **LEO Orbit:** Success appears correlated with the number of flights.
- **GTO Orbit:** No clear relationship between flight number and success.

### Findings:
- **PO, LEO, ISS Orbits:** Higher payload mass corresponds to a more successful landing.
- **GTO Orbit:** Payload mass does not clearly affect landing success.

### 🔎 Launch Success Yearly Trend:
- **2013 to 2020:** Consistent increase in success rates.
- **Highlight:** SpaceX's continual improvement in launch reliability.

### 4.3. Interactive Map

**Objectives:**
- Mark all launch sites on a Folium map.
- Mark the success/failed launches for each site on the map.
- Calculate the distances between a launch site and its proximities.

**Findings:**
- **Geographical Insights:**
  - Many launch sites are situated near the Equator to leverage Earth's rotational speed, reducing fuel needs and enhancing cost-efficiency.
  - Coastal locations are predominant to ensure safe disposal of rocket stages into the ocean, reducing risks to populated areas and enabling efficient transportation and assembly of rocket components.
  - Launch sites are strategically located near coastlines to avoid overflight of populated areas and enable water landing in case of an abort.
  - Proximity to highways and railways ensures efficient transport of cargo and personnel.
  - Remote launch site locations reduce risk to densely populated areas, with limited infrastructure nearby.

### 4.4. Interactive Dashboard

**Objective:**
- Build a Plotly Dash application for users to perform interactive visual analytics on SpaceX launch data in real-time.

👉 The dashboard application contains input components to interact with a pie chart and a scatter plot chart.

**Findings:**
- **Launch Success Insights:**
  - The site with the largest number of successful launches is **KSC LC-39A**, accounting for **41.7%** of total successful launches.
  - **KSC LC-39A** also has the highest launch success rate, at **76.9%**.
  - The highest launch success rate is associated with payloads between **3,000 Kg and 4,000 Kg**.
  - The lowest launch success rate occurs with payloads between **6,000 Kg and 8,000 Kg**.
  - The **FT Booster** version appears to have the highest launch success rate.

## 5. Predictive Analysis

**Objectives:**
- Perform exploratory data analysis and determine training labels.
- Create a column for the class (1 for successful landing, 0 for failure).
- Standardize the data and split it into training and testing sets.
- Find the best hyperparameters for SVM, Classification Trees, and Logistic Regression.
- Identify the method that performs best using test data.

### 5.1. Logistic Regression Model (LR)
- **Create GridSearchCV Object:** Perform grid search to find the best parameters for the Logistic Regression model.
  - Output the **GridSearchCV** object for logistic regression.
  - Display the best parameters using the `best_params_` attribute and the accuracy on the validation data using the `best_score_` attribute.

### 5.2. Support Vector Machine Model (SVM)
- **Create GridSearchCV Object:** Perform grid search to find the best parameters for the SVM model.
  - Output the **GridSearchCV** object for SVM.
  - Display the best parameters using the `best_params_` attribute and the accuracy on the validation data using the `best_score_` attribute.

### 5.3. Decision Tree Model (DT)
- **Create GridSearchCV Object:** Perform grid search to find the best parameters for the Decision Tree model.
  - Output the **GridSearchCV** object for Decision Tree.
  - Display the best parameters using the `best_params_` attribute and the accuracy on the validation data using the `best_score_` attribute.

### 5.4. K-Nearest Neighbors Model (KNN)
- **Create GridSearchCV Object:** Perform grid search to find the best parameters for the KNN model.
  - Output the **GridSearchCV** object for KNN.
  - Display the best parameters using the `best_params_` attribute and the accuracy on the validation data using the `best_score_` attribute.

## 6. Conclusion

### 6.1. Comparing Models Performance
- **Accuracy:** All models (Logistic Regression, SVM, Decision Tree, and KNN) achieved an accuracy of **83.33%** on the test data.
- **Challenge:** All models faced a challenge with false positives in their confusion matrices.
- **Recommendation:** Further analysis and fine-tuning are required to mitigate false positives and enhance overall model performance.

### 6.2. Interpreting Results
- **True Positives (TP):** The model correctly predicted successful first-stage landings **12 times**.
- **False Positives (FP):** The model incorrectly predicted successful landings **3 times** when they were unsuccessful.
- **True Negatives (TN):** The model correctly predicted unsuccessful landings **3 times**.
- **False Negatives (FN):** The model did not incorrectly predict unsuccessful landings.

### 6.3. Results

**Key Takeaways:**
- Four machine learning models (Logistic Regression, SVM, Decision Tree, KNN) were employed.
- All models achieved an identical accuracy of **83.33%** on the test data.
- The analysis highlighted a common issue: **false positives** present in all models.

**Implications:**
- Predictive models play a vital role in estimating launch costs.
- Addressing the challenge of false positives is crucial for enhancing cost prediction accuracy and ensuring more reliable mission predictions.
