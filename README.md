# 🧹 Excel Data Cleaning Automation Tool

## 📊 Overview

This project is a **browser-based Excel data cleaning tool** that automatically detects and resolves common data quality issues. All processing runs locally in the browser, ensuring that sensitive data remains on the user’s machine and is never transmitted to external servers.

The application was developed in Replit through multiple iterations, starting from an initial set of data cleaning rules that I defined and progressively refined into a comprehensive, rule-based data quality framework capable of handling real-world inconsistencies and edge cases.

---

## 🚀 Live Demo

🌐 https://constantinescu.github.io/excel-data-cleaning-automation/

---

## 🎯 Key Features

### 🔹 Automated Data Cleaning

* Trims whitespace across all columns
* Normalizes text casing
* Detects and flags missing values
* Standardizes date formats to ISO (`YYYY-MM-DD`)
* Normalizes number formats (European & international)

### 🔹 Intelligent Detection

* Identifies duplicate rows and columns
* Detects inconsistent values (e.g. YES / yes / Yes)
* Automatically classifies column types (text, number, date, boolean)

### 🔹 Smart Normalization Rules

* Boolean normalization → `Yes / No / N/A`
* Email normalization → lowercase
* Name cleanup (removes invalid characters, preserves hyphens/apostrophes)
* Geographic normalization (city/country capitalization + abbreviation expansion)

### 🔹 User-Controlled Cleaning

* Select which transformations to apply
* Filter issues by type or column
* Preview all changes before applying

### 🔹 Data Transparency

* Side-by-side comparison (original vs cleaned data)
* Export full **data dictionary** with:

  * Original values
  * Cleaned values
  * Applied rules

---

## 🧠 Cleaning Logic

The tool applies a structured rule system covering:

* Data normalization
* Type detection
* Value standardization
* Domain-specific cleaning (names, emails, locations)

📄 Full rule set available here:
👉 `CLEANING_RULES.md`

---

## ⚙️ How It Works

1. **Upload** Excel file
2. **Analyze** dataset automatically
3. **Review** detected issues
4. **Compare** original vs cleaned data
5. **Apply** selected transformations
6. **Export** cleaned dataset + data dictionary

---

## 🛠️ Tech Stack

* HTML / CSS / React/JavaScript
* Excel file processing (client-side)
* Hosted via GitHub Pages or Replit

---

## 📁 Project Structure

```id="projstruct"
├── index.html
├── flower.jpg
├── CLEANING_RULES.md
├── sample_input.xlsx
├── sample_output.xlsx
```

---

## 📸 Screenshots

*(Add screenshots here )*

* Upload step
* Data preview
* Cleaning suggestions
* Before vs After comparison

---

## 💡 Use Cases

* Cleaning survey data
* Preparing datasets for analysis
* Standardizing CRM exports
* Fixing inconsistent Excel files

---

## 🎓 What I Learned

* Designing rule-based data cleaning systems
* Handling messy real-world datasets
* Building user-friendly data tools with AI support
* Structuring data transformation pipelines

---

## 🔮 Future Improvements

* Add custom rule editor
* Support for larger datasets
* Integration with cloud storage
* More advanced anomaly detection
