# MIMIC-III Heart Failure Analysis  
Comorbidities, Cohort Construction, and Regex Extraction from Clinical Notes (R/SQL)

---

## Project Overview

This repository contains an R-based analysis of heart failure (HF) in the MIMIC-III database.

The notebook covers:

1. Heart failure comorbidities (co-occurring diseases)
2. Building a heart failure cohort (primary diagnosis)
3. Regular expressions (regex) on clinical text:
   - Medication extraction from discharge summaries
   - Comparison with structured prescriptions
4. Heart failure ejection fraction (LVEF):
   - Regex extraction of LVEF from echocardiogram reports in NOTEEVENTS
   - Adding LVEF to the HF cohort
   - LVEF distribution overall and stratified by gender and hypertension

No patient-level data is included in this repository.

---

## Data Source

MIMIC-III v1.4 (credentialed access required via PhysioNet).

The notebook connects to a MIMIC-III database and queries structured and unstructured EHR tables.

---

## Tools and Libraries (R)

The notebook uses R and connects to the database via `RMariaDB`.

Key libraries loaded in the notebook include:

- dplyr, tidyr, tibble
- lubridate, readr
- stringr
- ggplot2
- data.table
- odbc
- RMariaDB

---

# 1) Heart Failure: Comorbidities

## Goal

Explore comorbidities (diseases co-occurring with a primary condition) in the context of heart failure.

The notebook queries diagnoses in relation to heart failure and includes a written clinical interpretation of patterns observed in the outputs.

---

# 2) Heart Failure: Building a Cohort

## Goal

Construct a cohort of admissions where heart failure is the **primary diagnosis**.

SQL is used to build and store the resulting HF cohort as a data frame for later steps.

---

# 3) Regular Expressions (Clinical Text Mining)

## 3.1 Extract a Discharge Summary Text

The notebook retrieves the `TEXT` field from `NOTEEVENTS` for a specific discharge summary record:

- `SUBJECT_ID = 13702`
- `CATEGORY = "Discharge summary"`
- `CHARTDATE = 2118-06-14`

---

## 3.2 Medication Extraction (Regex)

Regex is applied to extract admission medications from the discharge summary text.

The notebook structures extracted results into a data frame containing:

- medication name
- dosage
- units

The notebook documents typical formats that work well and cases where the approach fails due to more complex or uncommon medication text patterns.

---

## 3.3 Comparison with PRESCRIPTIONS

Medications are also available in a structured MIMIC-III table.

The notebook compares the regex-extracted results against the `PRESCRIPTIONS` table for the same admission and notes discrepancies and limitations of matching unstructured text to structured prescription records.

---

# 4) Heart Failure: Ejection Fraction (LVEF)

## 4.1 Motivation

The notebook introduces ejection fraction and focuses on extracting **LVEF** values reported in echocardiogram notes.

---

## 4.2 Regex Extraction of LVEF from NOTEEVENTS

LVEF is extracted using regular expressions from echocardiogram reports associated with admissions where the **primary diagnosis is heart failure**.

The notebook explicitly accounts for heterogeneous documentation patterns (e.g., different ways LVEF is written).

---

## 4.3 Add LVEF to the HF Cohort

Extracted LVEF values are aggregated at the admission level (`HADM_ID`), and if multiple measurements exist for the same admission, the average value is used.

The resulting LVEF-enriched cohort is then used for downstream summaries and plots.

---

## 4.4 LVEF Distribution and Stratifications

The notebook produces histograms showing:

- The distribution of LVEF values (with handling of outliers)
- LVEF distribution stratified by:
  - gender
  - presence of hypertension

---

## Notes

- This repository contains only code, queries, and documentation.
- No identifiable patient information is shared.
- Running the notebook requires credentialed access to MIMIC-III.
