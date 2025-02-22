# 🏠 Paris Agent Deduplication Challenge  

![Python](https://img.shields.io/badge/Python-3.12-blue?logo=python&logoColor=white)  
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-orange?logo=pandas)  
![MongoDB](https://img.shields.io/badge/MongoDB-NoSQL-green?logo=mongodb)  

## 📌 Overview  
This repository contains a solution for the **Paris Agent Deduplication Challenge**. The goal is to **clean and deduplicate** a dataset of real estate agents sourced from multiple platforms in Paris.  

The dataset, stored in **MongoDB**, consists of **nested documents** with various attributes such as:  
- **Agent Name**  
- **Phone Numbers**  
- **Email Addresses**  
- **Company Names**  
- **Addresses**  

## 📑 Table of Contents  
- [Methodology](#methodology)  
- [Assumptions Made](#assumptions-made)  
- [Data Quality Issues](#data-quality-issues)  
- [Recommendations](#recommendations)  
- [Conclusion](#conclusion)  
- [Installation & Usage](#installation--usage)  
- [Dependencies](#dependencies)  
- [Contributors](#contributors)  
- [License](#license)  

## 🛠 Methodology  

We explored **four different approaches** for deduplication:  

### 1️⃣ **Pandas `drop_duplicates()` Method**  
- **Approach**: Flatten nested documents into columns and apply Pandas' `drop_duplicates()` function.  
- **Limitation**:  
  - Struggles with high-dimensional datasets due to excessive column expansion.  
  - Not ideal for deeply nested structures.  

### 2️⃣ **MongoDB `ensureIndex()` Method**  
- **Approach**:  
  - Create a **unique compound index** on key fields (**Agent name, Phone numbers, Email addresses**) using MongoDB's **`ensureIndex()`** with `dropDups=True`.  
- **Advantage**:  
  - Utilizes **MongoDB’s indexing mechanism** to drop duplicates automatically.  
- **Note**:  
  - `dropDups` is **deprecated in MongoDB 3.0+** and is used here for **demonstration purposes only**.  

### 3️⃣ **Hashing Approach**  
- **Approach**: Generate **hash values** for relevant fields. If two records share the same hash, one is removed.  
- **Advantage**:  
  - **Fast and efficient** for large datasets.  
- **Limitation**:  
  - Sensitive to minor variations (e.g., extra spaces, punctuation).  

### 4️⃣ **Fuzzy Matching with RapidFuzz**  
- **Approach**:  
  - Use **RapidFuzz** to compute similarity scores for text fields (e.g., agent names).  
  - If **similarity score > 90%**, records are considered duplicates.  
- **Advantage**:  
  - Captures **near-duplicates** despite typos or formatting inconsistencies.  
- **Limitation**:  
  - Computationally expensive on large datasets.  

---

## 🔎 Assumptions Made  
✅ **Flattening Nested Data**: Used `pandas.json_normalize()` to extract key fields from **nested MongoDB documents**.  
✅ **Uniformity in Data**: Assumed **cleaned fields** would provide a reliable basis for matching duplicates.  

---

## ⚠️ Data Quality Issues  

### 🔹 **Completeness**  
- Several fields contain **missing or incomplete data**, affecting matching accuracy.  

### 🔹 **City Column Inconsistencies**  
- "Paris" appears in multiple formats:  
  - `"PARIS"` (uppercase)  
  - `"paris"` (lowercase)  
  - `"Paris, 75001"` (with postal code)  

### 🔹 **Address Anomalies**  
- Some records contain placeholders like **"------"** → Should be treated as missing values.  

### 🔹 **General Inconsistencies**  
- **Punctuation, extra spaces, and formatting variations** complicate deduplication.  

---

## ✅ Recommendations  

### 🔹 **Enhanced Data Cleaning**  
🔹 **Normalize city names and addresses** during data ingestion.  
🔹 Replace **placeholder values ("------")** with **empty/null values**.  

### 🔹 **Metadata Collection**  
🔹 Store **platform-specific identifiers** (e.g., **social media handles, unique IDs**) to improve duplicate detection.  

### 🔹 **Advanced Techniques**  
🔹 Implement **machine learning–based entity resolution** to handle ambiguous cases.  
🔹 Use **parallel processing** to optimize performance for large datasets.  

### 🔹 **Evaluation Metrics**  
🔹 Track **precision, recall, and F1-score** to assess deduplication effectiveness.  
🔹 Use **visualizations** to monitor improvement over time.  

---

## 🎯 Conclusion  
This project evaluates multiple deduplication techniques, each with **unique strengths and trade-offs**.  
- **Exact methods** (Pandas, MongoDB indexing, hashing) are **efficient but strict**.  
- **Fuzzy methods** (RapidFuzz) handle **near-duplicates** but can be **computationally intensive**.  

By **combining multiple techniques** and **enhancing data quality**, this solution provides a **scalable** approach to deduplication.  

---

## 💻 Installation & Usage  

### 🛠 Prerequisites  
Ensure you have the following installed:  
- **Python (3.12)**
- **MongoDB**
- **Pandas**
- **RapidFuzz**

### 🔹 Clone the Repository  
```bash
git clone https://github.com/yourusername/paris-agent-deduplication.git
cd paris-agent-deduplication
