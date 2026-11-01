# Car Sales Analytics Dashboard (AutoInsight)
**Course:** Year 3, Term 1 — Project Management  
**Team 10 (DS Group 1):**
* **Chhoun Vannak Sok Panha** — Scrum Master
* **Samolty Lundy** — Lead Developer
* **Por Menghong** — Product Owner

---

## 📌 Project Overview
The **Car Sales Analytics Dashboard** is designed to transform unstructured and manual used car market listings into an automated, visual decision-making system for dealership managers. The product roadmap spans three 2-week Agile Scrum sprints:
* **Sprint 1 (Weeks 1–2):** Data Preparation & Quality Assurance (19 Oct – 02 Nov) — *Current Scope*
* **Sprint 2 (Weeks 3–4):** SQL Analysis & Business Insights
* **Sprint 3 (Weeks 5–6):** 4-Page Power BI Management Dashboard Delivery

---

## 🚀 Sprint 1: Data Prep & Validation (`CDS-64` to `CDS-68`)
**Goal:** Ingest, inspect, audit data quality, sanitize, and transform raw car sales records into a validated, production-grade analytical dataset.

### Jira Traceability Matrix (Total: 23 Story Points)
| Jira Ticket | Epic | Description | Story Points | Status |
| :--- | :--- | :--- | :---: | :---: |
| **CDS-64** | Data Collection | Acquire, inspect provenance, and verify raw dataset intake | 2 SP | Done |
| **CDS-65** | Data Understanding | Profile schema, variable types, distributions, and domain semantics | 3 SP | Done |
| **CDS-66** | Data Quality | Assess missing data, statistical outliers (IQR, Z-score), and duplicates | 5 SP | Done |
| **CDS-67** | Data Cleaning | Impute missing values and filter erroneous domain-impossible records | 8 SP | Done |
| **CDS-68** | Data Transformation | Retain core BI dimensions, drop noise artifacts, and export clean CSV | 5 SP | Done |

---

## 📂 Repository Structure
```text
CarSales_Dashboard/
├── raw/
│   └── cardekho_raw.csv         # Untouched raw intake dataset (15,244 rows, 25 columns)
├── cleaned_data/
│   └── cleaned.csv              # Production cleaned dataset (15,231 rows, 22 columns)
├── data_cleaning.ipynb          # End-to-end reproducible data preparation pipeline
├── .gitignore                   # Git exclusion rules
└── README.md                    # Project documentation and sprint audit log
```

---

## 🛠️ Data Pipeline Summary & Technical Decisions

1. **Ingestion (`CDS-64`):**
   * Loaded raw dataset from `raw/cardekho_raw.csv` containing 15,244 listings.
2. **Quality Audit & Imputation (`CDS-66`, `CDS-67`):**
   * Identified 13 physically impossible odometer records with annual usage exceeding 100,000 km/year (e.g. 5-year-old vehicle with 3,800,000 km). These clerical errors were filtered out.
   * Legitimate high-end luxury vehicle prices (Rolls-Royce, Bentley, Ferrari) and multi-seat MPVs (6–9 seats) were intentionally preserved.
   * `real_avg_owner_count` (~1.99% missing) imputed with the robust median (`1.18`).
   * `real_new_ex_showroom_price` (~34.90% missing) imputed hierarchically by `brand` median, falling back to `brand_tier` median, preserving price boundaries across market tiers.
3. **Feature Strategy (`CDS-68`):**
   * **Retained Core Dimensions:** `brand`, `model`, `fuel_type`, `seats`, `real_primary_city`, `seller_type`, `transmission_type`, `selling_price`, `vehicle_age`, `km_driven`, `mileage`, `engine`, `max_power`, `real_body_type`, `brand_tier`. These are vital for Sprint 2 SQL queries and Sprint 3 Power BI slicers.
   * **Pruned Artifacts:** Dropped constant web scraping flags with zero information gain (`matched_dt`, `matched_eng`, `real_model_variant_count`).
4. **Final Deliverable:**
   * Cleaned dataset exported to `cleaned_data/cleaned.csv` with **15,231 records**, **22 features**, **0 missing values**, and **0 duplicates**.

---

## 🔄 Reproduction Instructions
To re-run the entire data preparation pipeline from scratch:
```bash
# 1. Open the project directory
cd CarSales_Dashboard

# 2. Execute the notebook via Jupyter
jupyter nbconvert --to notebook --execute --inplace data_cleaning.ipynb
```
The output file `cleaned_data/cleaned.csv` will be automatically updated and validated.
