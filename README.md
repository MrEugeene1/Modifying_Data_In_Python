<div align="center">

# 📍 Modifying Data with Pandas

**A hands-on notebook for cleaning and transforming real retail data with pandas**

![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-Data%20Wrangling-150458?logo=pandas&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)
![Level](https://img.shields.io/badge/Level-Beginner-brightgreen)

</div>

---

## 📖 Overview

Real-world data is rarely clean. It has **missing values**, **inconsistent labels**, columns in the **wrong units**, and **text categories** that machine learning models can't use.

This notebook works through each of these problems on the **Big Mart Sales** dataset and shows how to fix them step by step with pandas.

---

## 📚 Table of Contents

| # | Topic | Key Tools |
|---|-------|-----------|
| 1 | [Imputing missing values](#1--imputing-missing-values) | `isna()`, `loc`, `mean()`, `mode()`, `fillna()` |
| 2 | [Updating values with a mapping](#2--updating-values-with-a-mapping) | `value_counts()`, dictionaries, `map()` |
| 3 | [Creating new columns from existing ones](#3--creating-new-columns-from-existing-ones) | `apply()`, `lambda`, custom functions |
| 4 | [Converting categorical to numerical](#4--converting-categorical-to-numerical) | `get_dummies()` (One-Hot Encoding) |

---

## 🗂️ Project Structure

```
Modifying Data/
├── 4. Modifying Data.ipynb      # Main notebook
├── README.md                    # You are here
└── datasets/
    └── big_mart_sales.csv       # Big Mart Sales dataset
```

---

## 🛒 Dataset

**Big Mart Sales**: product and outlet records from a retail chain.

| Property | Value |
|----------|-------|
| Rows | 8,523 |
| Columns | 12 |
| Missing values | `Item_Weight` (1,463) · `Outlet_Size` (2,410) |

<details>
<summary><b>View all columns</b></summary>

| Column | Description |
|--------|-------------|
| `Item_Identifier` | Unique product ID |
| `Item_Weight` | Weight of the product |
| `Item_Fat_Content` | Low fat or regular |
| `Item_Visibility` | % of display area given to the product |
| `Item_Type` | Product category |
| `Item_MRP` | Maximum retail price |
| `Outlet_Identifier` | Unique store ID |
| `Outlet_Establishment_Year` | Year the store opened |
| `Outlet_Size` | Store size (Small / Medium / High) |
| `Outlet_Location_Type` | City tier of the store |
| `Outlet_Type` | Grocery store or supermarket type |
| `Item_Outlet_Sales` | Sales of the product at that store |

</details>

---

## 🧠 What You'll Learn

### 1 · Imputing Missing Values

Find the gaps, then fill them using the right strategy for the column type.

```python
# How many values are missing in each column?
data.isna().sum()

# Numeric column → fill with the MEAN
data.loc[data.Item_Weight.isna(), 'Item_Weight'] = data.Item_Weight.mean()

# Categorical column → fill with the MODE (most frequent value)
data.loc[data.Outlet_Size.isna(), 'Outlet_Size'] = 'Medium'
```

> 💡 **Rule of thumb:** use the **mean** (or median) for numbers and the **mode** for categories.

---

### 2 · Updating Values with a Mapping

`Item_Fat_Content` has **5 spellings** for only **2 real categories**:

| Before | After |
|--------|-------|
| `Low Fat`, `LF`, `low fat` | `LF` |
| `Regular`, `reg` | `R` |

```python
mapping = {
    'Low Fat': 'LF',
    'Regular': 'R',
    'LF'     : 'LF',
    'reg'    : 'R',
    'low fat': 'LF'
}

data.Item_Fat_Content = data.Item_Fat_Content.map(mapping)
```

> ⚠️ `.map()` turns any value **not** in the dictionary into `NaN`. That's why `'LF': 'LF'` is included. Run this cell **only once**, because a second run would turn every `'R'` into `NaN`.

---

### 3 · Creating New Columns from Existing Ones

Use `apply()` to run a function on every value in a column.

```python
# Quick one-liner with a lambda
data.Item_MRP.apply(lambda x: x / 74)

# Reusable custom function
def convert(price):
    price = price / 74
    price = price + 1.28
    return price

data['Item_MRP_in_USD_UPDATED'] = data.Item_MRP.apply(convert)
```

---

### 4 · Converting Categorical to Numerical

Most machine learning algorithms only understand numbers. **One-Hot Encoding** creates a binary (0/1) column for each category.

```python
data = pd.get_dummies(data)
```

| Gender | → | Gender_Female | Gender_Male |
|--------|---|:---:|:---:|
| Male   | → | 0 | 1 |
| Female | → | 1 | 0 |

> 📈 The DataFrame grows from **13 → 1,603 columns**, mostly because ID columns like `Item_Identifier` have many unique values. In real projects, drop ID columns before encoding.

---

## 🚀 Getting Started

### Prerequisites

- Python 3.x
- Jupyter Notebook, JupyterLab, or VS Code with the Jupyter extension

### Installation

```bash
# 1. Install the dependencies
pip install pandas jupyter

# 2. Open the folder
cd "Modifying Data"

# 3. Launch the notebook
jupyter notebook "4. Modifying Data.ipynb"
```

Then run the cells **from top to bottom** (`Shift + Enter`).

---

## 📝 Quick Reference

| Task | Code |
|------|------|
| Count missing values | `df.isna().sum()` |
| Select rows by condition | `df.loc[condition, 'column']` |
| Fill missing values | `df['col'] = df['col'].fillna(value)` |
| Most frequent value | `df['col'].mode()[0]` |
| Count each category | `df['col'].value_counts()` |
| Replace values via dictionary | `df['col'].map(mapping)` |
| Transform every value | `df['col'].apply(func)` |
| One-hot encode | `pd.get_dummies(df)` |
| Check rows × columns | `df.shape` |

---

## ⚠️ Notes

- **Re-running cells:** some cells (such as `map` and `get_dummies`) change `data` in place. If results look wrong, restart the kernel and run everything again from the top.

---

<div align="center">

**⭐ Happy Learning! ⭐**

*Clean data → Better insights → Better models*

</div>
\