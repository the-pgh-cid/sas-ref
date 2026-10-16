# SAS Code Analysis Summary

**Directory:** `../adrec_expansion`  
**Files Analyzed:** 350  
**Generated:** 2026-10-05 17:39:59

---

## Overall Summary

| Step Type | Count |
|-----------|-------|
| **DATA Steps** | 3193 |
| **PROC Steps** | 2308 |
| **PROC Types** | |
|   - PROC APPEND | 58 |
|   - PROC CONTENTS | 70 |
|   - PROC DATASETS | 73 |
|   - PROC EXPORT | 33 |
|   - PROC FORMAT | 42 |
|   - PROC FREQ | 373 |
|   - PROC GEOCODE | 6 |
|   - PROC GINSIDE | 1 |
|   - PROC IMPORT | 9 |
|   - PROC MAPIMPORT | 1 |
|   - PROC MEANS | 116 |
|   - PROC PRINT | 148 |
|   - PROC PRINTTO | 2 |
|   - PROC RANK | 4 |
|   - PROC SGPLOT | 39 |
|   - PROC SORT | 1078 |
|   - PROC SQL | 222 |
|   - PROC SUMMARY | 1 |
|   - PROC SURVEYSELECT | 4 |
|   - PROC TABULATE | 7 |
|   - PROC TEMPLATE | 1 |
|   - PROC TRANSPOSE | 20 |
| **Macros Defined** | 456 |
| **Macro Calls** | 345 |
| **Total Lines of Code** | 53,188 |

### Step Categories

| Category | Count | Description |
|----------|-------|-------------|
| **Data Management:Transformation** | 2796 | Sorting, transposing, data structure changes |
| **Data Management:Join** | 862 | Merging datasets, combining records |
| **Analyses:Descriptive** | 490 | Means, frequencies, univariate statistics |
| **Data Management:Filter** | 438 | Subsetting, conditional selection |
| **Data Management:Query** | 222 | SQL queries and operations |
| **Data Management:Metadata** | 185 | Dataset info, formats, catalogs |
| **Reporting:Tables** | 155 | Print, report, tabulate output |
| **Data Management:Generation** | 140 | Creating data from scratch (literals, loops) |
| **Analyses:Aggregation** | 113 | Summarization, grouping, BY-group processing |
| **Integration:File Io** | 44 | Import/export flat files |
| **Reporting:Visualization** | 39 | Charts, graphs, plots, quality control |
| **Analyses:Spatial** | 8 |  |
| **Analyses:Standardization** | 4 | Standardization, ranking procedures |
| **Analyses:Sampling** | 4 |  |
| **Reporting:Ods** | 1 | ODS templates, documents, output delivery |


## Overall Complexity Distribution

| Complexity | Count | Proportion |
|------------|-------|------------|
| **`H`igh** | 240 | 4.4% |
| **`M`edium** | 1749 | 31.8% |
| **`L`ow** | 3512 | 63.8% |
| **Total** | 5501 | 100.0% |

### Examples

#### `H` Complexity Example
**File:** `add_blocks.sas` | **Line:** 42
**Type:** DATA Step `work.zip9_impute`
**Category:** Data Management:Join
**Operation:** MERGE
**Details:** Inputs: work.remainder, geo.zip9_block_shares | Outputs: work.zip9_impute, work.remainder | BY: zip5, zip4 | Contains MERGE

#### `M` Complexity Example
**File:** `add_blocks.sas` | **Line:** 25
**Type:** DATA Step `work.remainder`
**Category:** Data Management:Join
**Operation:** MERGE
**Details:** Inputs: pnmgeo.pnm_prep_&_ds_suffix_., work.succ_ids | Outputs: work.remainder | BY: &unique_obs_var_list. | Contains MERGE

#### `L` Complexity Example
**File:** `add_blocks.sas` | **Line:** 12
**Type:** PROC FORMAT
**Category:** Data Management:Metadata


## Subfolder Summaries

### [DIR] `ar_pnm_geo/`

**Files:** 2 | **Lines of Code:** 344

| Step Type | Count |
|-----------|-------|
| DATA Steps | 13 |
| PROC Steps | 22 |
| **PROC Types** | |
|   - PROC FORMAT | 1 |
|   - PROC FREQ | 4 |
|   - PROC PRINT | 2 |
|   - PROC SORT | 15 |
| Macros Defined | 4 |
| Macro Calls | 8 |

**Step Categories:**

| Category | Count |
|----------|-------|
| Data Management:Transformation | 16 |
| Data Management:Join | 9 |
| Analyses:Descriptive | 4 |
| Data Management:Filter | 2 |
| Reporting:Tables | 2 |
| Data Management:Metadata | 1 |
| Analyses:Aggregation | 1 |

**Complexity:**
- `H`: 6 (17.1%)
- `M`: 6 (17.1%)
- `L`: 23 (65.7%)

### [DIR] `ar_source_pp/`

**Files:** 2 | **Lines of Code:** 149

| Step Type | Count |
|-----------|-------|
| DATA Steps | 9 |
| PROC Steps | 10 |
| **PROC Types** | |
|   - PROC DATASETS | 1 |
|   - PROC FREQ | 1 |
|   - PROC SORT | 8 |
| Macros Defined | 1 |
| Macro Calls | 6 |

**Step Categories:**

| Category | Count |
|----------|-------|
| Data Management:Transformation | 10 |
| Data Management:Join | 5 |
| Data Management:Filter | 2 |
| Data Management:Metadata | 1 |
| Analyses:Descriptive | 1 |

**Complexity:**
- `H`: 2 (10.5%)
- `M`: 7 (36.8%)
- `L`: 10 (52.6%)

### [DIR] `ar_xwalks/`

**Files:** 1 | **Lines of Code:** 26

| Step Type | Count |
|-----------|-------|
| DATA Steps | 0 |
| PROC Steps | 3 |
| **PROC Types** | |
|   - PROC CONTENTS | 1 |
|   - PROC FORMAT | 1 |
|   - PROC FREQ | 1 |
| Macros Defined | 0 |
| Macro Calls | 1 |

**Step Categories:**

| Category | Count |
|----------|-------|
| Data Management:Metadata | 2 |
| Analyses:Descriptive | 1 |

**Complexity:**
- `H`: 0 (0.0%)
- `M`: 0 (0.0%)
- `L`: 3 (100.0%)

### [DIR] `citizenship/`

**Files:** 2 | **Lines of Code:** 136

| Step Type | Count |
|-----------|-------|
| DATA Steps | 8 |
| PROC Steps | 15 |
| **PROC Types** | |
|   - PROC CONTENTS | 2 |
|   - PROC FORMAT | 1 |
|   - PROC FREQ | 4 |
|   - PROC MEANS | 1 |
|   - PROC PRINT | 2 |
|   - PROC SORT | 5 |
| Macros Defined | 0 |
| Macro Calls | 0 |

**Step Categories:**

| Category | Count |
|----------|-------|
| Data Management:Transformation | 11 |
| Analyses:Descriptive | 5 |
| Data Management:Metadata | 3 |
| Data Management:Join | 2 |
| Reporting:Tables | 2 |

**Complexity:**
- `H`: 1 (4.3%)
- `M`: 3 (13.0%)
- `L`: 19 (82.6%)

### [DIR] `config/`

**Files:** 3 | **Lines of Code:** 296

| Step Type | Count |
|-----------|-------|
| DATA Steps | 2 |
| PROC Steps | 4 |
| **PROC Types** | |
|   - PROC CONTENTS | 1 |
|   - PROC FORMAT | 1 |
|   - PROC FREQ | 1 |
|   - PROC TEMPLATE | 1 |
| Macros Defined | 6 |
| Macro Calls | 0 |

**Step Categories:**

| Category | Count |
|----------|-------|
| Data Management:Generation | 2 |
| Data Management:Metadata | 2 |
| Analyses:Descriptive | 1 |
| Reporting:Ods | 1 |

**Complexity:**
- `H`: 0 (0.0%)
- `M`: 0 (0.0%)
- `L`: 6 (100.0%)

### [DIR] `demographics/`

**Files:** 100 | **Lines of Code:** 19,808

| Step Type | Count |
|-----------|-------|
| DATA Steps | 1806 |
| PROC Steps | 67 |
| **PROC Types** | |
|   - PROC APPEND | 2 |
|   - PROC CONTENTS | 5 |
|   - PROC DATASETS | 6 |
|   - PROC FORMAT | 9 |
|   - PROC FREQ | 11 |
|   - PROC MEANS | 1 |
|   - PROC SORT | 24 |
|   - PROC SQL | 7 |
|   - PROC TABULATE | 2 |
| Macros Defined | 77 |
| Macro Calls | 92 |

**Step Categories:**

| Category | Count |
|----------|-------|
| Data Management:Transformation | 1038 |
| Data Management:Join | 407 |
| Data Management:Filter | 288 |
| Analyses:Aggregation | 55 |
| Data Management:Generation | 44 |
| Data Management:Metadata | 20 |
| Analyses:Descriptive | 12 |
| Data Management:Query | 7 |
| Reporting:Tables | 2 |

**Complexity:**
- `H`: 56 (3.0%)
- `M`: 963 (51.4%)
- `L`: 854 (45.6%)

### [DIR] `filtering/`

**Files:** 20 | **Lines of Code:** 2,711

| Step Type | Count |
|-----------|-------|
| DATA Steps | 125 |
| PROC Steps | 191 |
| **PROC Types** | |
|   - PROC APPEND | 3 |
|   - PROC CONTENTS | 5 |
|   - PROC FORMAT | 4 |
|   - PROC FREQ | 89 |
|   - PROC IMPORT | 2 |
|   - PROC MEANS | 17 |
|   - PROC PRINT | 8 |
|   - PROC SORT | 59 |
|   - PROC SQL | 4 |
| Macros Defined | 13 |
| Macro Calls | 12 |

**Step Categories:**

| Category | Count |
|----------|-------|
| Data Management:Transformation | 134 |
| Analyses:Descriptive | 106 |
| Data Management:Join | 28 |
| Data Management:Filter | 21 |
| Data Management:Metadata | 9 |
| Reporting:Tables | 8 |
| Data Management:Query | 4 |
| Analyses:Aggregation | 3 |
| Integration:File Io | 2 |
| Data Management:Generation | 1 |

**Complexity:**
- `H`: 10 (3.2%)
- `M`: 81 (25.6%)
- `L`: 225 (71.2%)

### [DIR] `final_product/`

**Files:** 6 | **Lines of Code:** 712

| Step Type | Count |
|-----------|-------|
| DATA Steps | 21 |
| PROC Steps | 36 |
| **PROC Types** | |
|   - PROC APPEND | 1 |
|   - PROC CONTENTS | 3 |
|   - PROC DATASETS | 1 |
|   - PROC FORMAT | 1 |
|   - PROC FREQ | 6 |
|   - PROC IMPORT | 1 |
|   - PROC MEANS | 2 |
|   - PROC SORT | 14 |
|   - PROC SQL | 6 |
|   - PROC TABULATE | 1 |
| Macros Defined | 11 |
| Macro Calls | 10 |

**Step Categories:**

| Category | Count |
|----------|-------|
| Data Management:Transformation | 22 |
| Analyses:Descriptive | 8 |
| Analyses:Aggregation | 6 |
| Data Management:Query | 6 |
| Data Management:Metadata | 5 |
| Data Management:Join | 5 |
| Data Management:Generation | 3 |
| Integration:File Io | 1 |
| Reporting:Tables | 1 |

**Complexity:**
- `H`: 3 (5.3%)
- `M`: 11 (19.3%)
- `L`: 43 (75.4%)

### [DIR] `fitness_for_use/`

**Files:** 37 | **Lines of Code:** 4,458

| Step Type | Count |
|-----------|-------|
| DATA Steps | 179 |
| PROC Steps | 347 |
| **PROC Types** | |
|   - PROC CONTENTS | 38 |
|   - PROC EXPORT | 5 |
|   - PROC FORMAT | 12 |
|   - PROC FREQ | 82 |
|   - PROC IMPORT | 4 |
|   - PROC MEANS | 4 |
|   - PROC PRINT | 27 |
|   - PROC SORT | 114 |
|   - PROC SQL | 61 |
| Macros Defined | 22 |
| Macro Calls | 17 |

**Step Categories:**

| Category | Count |
|----------|-------|
| Data Management:Transformation | 178 |
| Analyses:Descriptive | 86 |
| Data Management:Join | 63 |
| Data Management:Query | 61 |
| Data Management:Metadata | 50 |
| Data Management:Filter | 36 |
| Reporting:Tables | 27 |
| Data Management:Generation | 9 |
| Integration:File Io | 9 |
| Analyses:Aggregation | 7 |

**Complexity:**
- `H`: 22 (4.2%)
- `M`: 104 (19.8%)
- `L`: 400 (76.0%)

### [DIR] `geography/`

**Files:** 13 | **Lines of Code:** 1,730

| Step Type | Count |
|-----------|-------|
| DATA Steps | 66 |
| PROC Steps | 119 |
| **PROC Types** | |
|   - PROC CONTENTS | 2 |
|   - PROC DATASETS | 1 |
|   - PROC EXPORT | 2 |
|   - PROC FORMAT | 3 |
|   - PROC FREQ | 12 |
|   - PROC GEOCODE | 1 |
|   - PROC GINSIDE | 1 |
|   - PROC MAPIMPORT | 1 |
|   - PROC PRINT | 6 |
|   - PROC SORT | 76 |
|   - PROC SQL | 14 |
| Macros Defined | 19 |
| Macro Calls | 25 |

**Step Categories:**

| Category | Count |
|----------|-------|
| Data Management:Transformation | 101 |
| Data Management:Join | 30 |
| Data Management:Query | 14 |
| Analyses:Descriptive | 12 |
| Data Management:Filter | 7 |
| Reporting:Tables | 6 |
| Data Management:Metadata | 6 |
| Analyses:Spatial | 3 |
| Analyses:Aggregation | 3 |
| Integration:File Io | 2 |
| Data Management:Generation | 1 |

**Complexity:**
- `H`: 11 (5.9%)
- `M`: 34 (18.4%)
- `L`: 140 (75.7%)

### [DIR] `m_ar_source_pp/`

**Files:** 43 | **Lines of Code:** 5,998

| Step Type | Count |
|-----------|-------|
| DATA Steps | 192 |
| PROC Steps | 364 |
| **PROC Types** | |
|   - PROC APPEND | 43 |
|   - PROC CONTENTS | 2 |
|   - PROC DATASETS | 52 |
|   - PROC FORMAT | 2 |
|   - PROC FREQ | 7 |
|   - PROC MEANS | 6 |
|   - PROC PRINT | 36 |
|   - PROC SORT | 134 |
|   - PROC SQL | 77 |
|   - PROC TABULATE | 1 |
|   - PROC TRANSPOSE | 4 |
| Macros Defined | 96 |
| Macro Calls | 98 |

**Step Categories:**

| Category | Count |
|----------|-------|
| Data Management:Transformation | 252 |
| Data Management:Query | 77 |
| Data Management:Metadata | 56 |
| Data Management:Generation | 55 |
| Data Management:Join | 49 |
| Reporting:Tables | 37 |
| Analyses:Descriptive | 13 |
| Data Management:Filter | 12 |
| Analyses:Aggregation | 5 |

**Complexity:**
- `H`: 39 (7.0%)
- `M`: 75 (13.5%)
- `L`: 442 (79.5%)

### [DIR] `m_ar_xwalks/`

**Files:** 7 | **Lines of Code:** 2,053

| Step Type | Count |
|-----------|-------|
| DATA Steps | 116 |
| PROC Steps | 156 |
| **PROC Types** | |
|   - PROC APPEND | 5 |
|   - PROC DATASETS | 1 |
|   - PROC FREQ | 16 |
|   - PROC MEANS | 26 |
|   - PROC PRINT | 19 |
|   - PROC RANK | 1 |
|   - PROC SORT | 88 |
| Macros Defined | 10 |
| Macro Calls | 0 |

**Step Categories:**

| Category | Count |
|----------|-------|
| Data Management:Transformation | 146 |
| Data Management:Join | 48 |
| Analyses:Descriptive | 42 |
| Reporting:Tables | 19 |
| Data Management:Filter | 13 |
| Data Management:Generation | 1 |
| Analyses:Aggregation | 1 |
| Data Management:Metadata | 1 |
| Analyses:Standardization | 1 |

**Complexity:**
- `H`: 16 (5.9%)
- `M`: 87 (32.0%)
- `L`: 169 (62.1%)

### [DIR] `m_demoframe/`

**Files:** 2 | **Lines of Code:** 356

| Step Type | Count |
|-----------|-------|
| DATA Steps | 2 |
| PROC Steps | 0 |
| Macros Defined | 0 |
| Macro Calls | 0 |

**Step Categories:**

| Category | Count |
|----------|-------|
| Data Management:Generation | 2 |

**Complexity:**
- `H`: 0 (0.0%)
- `M`: 0 (0.0%)
- `L`: 2 (100.0%)

### [DIR] `m_geography/`

**Files:** 5 | **Lines of Code:** 2,019

| Step Type | Count |
|-----------|-------|
| DATA Steps | 34 |
| PROC Steps | 63 |
| **PROC Types** | |
|   - PROC DATASETS | 1 |
|   - PROC FREQ | 7 |
|   - PROC GEOCODE | 5 |
|   - PROC MEANS | 8 |
|   - PROC PRINT | 9 |
|   - PROC RANK | 1 |
|   - PROC SORT | 29 |
|   - PROC TRANSPOSE | 3 |
| Macros Defined | 33 |
| Macro Calls | 7 |

**Step Categories:**

| Category | Count |
|----------|-------|
| Data Management:Transformation | 44 |
| Analyses:Descriptive | 15 |
| Data Management:Join | 14 |
| Reporting:Tables | 9 |
| Data Management:Filter | 7 |
| Analyses:Spatial | 5 |
| Analyses:Standardization | 1 |
| Analyses:Aggregation | 1 |
| Data Management:Metadata | 1 |

**Complexity:**
- `H`: 9 (9.3%)
- `M`: 20 (20.6%)
- `L`: 68 (70.1%)

### [DIR] `m_ppm/`

**Files:** 34 | **Lines of Code:** 2,390

| Step Type | Count |
|-----------|-------|
| DATA Steps | 142 |
| PROC Steps | 169 |
| **PROC Types** | |
|   - PROC EXPORT | 6 |
|   - PROC RANK | 1 |
|   - PROC SORT | 146 |
|   - PROC SQL | 4 |
|   - PROC SURVEYSELECT | 2 |
|   - PROC TRANSPOSE | 10 |
| Macros Defined | 69 |
| Macro Calls | 2 |

**Step Categories:**

| Category | Count |
|----------|-------|
| Data Management:Transformation | 219 |
| Data Management:Join | 58 |
| Analyses:Aggregation | 12 |
| Data Management:Filter | 8 |
| Integration:File Io | 6 |
| Data Management:Query | 4 |
| Analyses:Sampling | 2 |
| Data Management:Generation | 1 |
| Analyses:Standardization | 1 |

**Complexity:**
- `H`: 18 (5.8%)
- `M`: 74 (23.8%)
- `L`: 219 (70.4%)

### [DIR] `one_off/`

**Files:** 32 | **Lines of Code:** 2,653

| Step Type | Count |
|-----------|-------|
| DATA Steps | 112 |
| PROC Steps | 159 |
| **PROC Types** | |
|   - PROC APPEND | 2 |
|   - PROC CONTENTS | 6 |
|   - PROC DATASETS | 5 |
|   - PROC EXPORT | 6 |
|   - PROC FORMAT | 3 |
|   - PROC FREQ | 29 |
|   - PROC MEANS | 10 |
|   - PROC PRINT | 8 |
|   - PROC SORT | 73 |
|   - PROC SQL | 17 |
| Macros Defined | 48 |
| Macro Calls | 22 |

**Step Categories:**

| Category | Count |
|----------|-------|
| Data Management:Transformation | 127 |
| Analyses:Descriptive | 39 |
| Data Management:Join | 36 |
| Data Management:Query | 17 |
| Data Management:Generation | 15 |
| Data Management:Metadata | 14 |
| Reporting:Tables | 8 |
| Data Management:Filter | 7 |
| Integration:File Io | 6 |
| Analyses:Aggregation | 2 |

**Complexity:**
- `H`: 11 (4.1%)
- `M`: 52 (19.2%)
- `L`: 208 (76.8%)

### [DIR] `person_id_xwalks/`

**Files:** 4 | **Lines of Code:** 2,670

| Step Type | Count |
|-----------|-------|
| DATA Steps | 191 |
| PROC Steps | 251 |
| **PROC Types** | |
|   - PROC APPEND | 1 |
|   - PROC CONTENTS | 4 |
|   - PROC DATASETS | 1 |
|   - PROC FORMAT | 4 |
|   - PROC FREQ | 37 |
|   - PROC MEANS | 32 |
|   - PROC PRINT | 20 |
|   - PROC SORT | 152 |
| Macros Defined | 1 |
| Macro Calls | 1 |

**Step Categories:**

| Category | Count |
|----------|-------|
| Data Management:Transformation | 263 |
| Analyses:Descriptive | 69 |
| Data Management:Join | 57 |
| Data Management:Filter | 22 |
| Reporting:Tables | 20 |
| Data Management:Metadata | 9 |
| Analyses:Aggregation | 2 |

**Complexity:**
- `H`: 22 (5.0%)
- `M`: 145 (32.8%)
- `L`: 275 (62.2%)

### [DIR] `pik_prop/`

**Files:** 3 | **Lines of Code:** 503

| Step Type | Count |
|-----------|-------|
| DATA Steps | 29 |
| PROC Steps | 57 |
| **PROC Types** | |
|   - PROC FREQ | 20 |
|   - PROC MEANS | 8 |
|   - PROC PRINT | 10 |
|   - PROC SORT | 18 |
|   - PROC SUMMARY | 1 |
| Macros Defined | 2 |
| Macro Calls | 2 |

**Step Categories:**

| Category | Count |
|----------|-------|
| Data Management:Transformation | 36 |
| Analyses:Descriptive | 29 |
| Data Management:Join | 11 |
| Reporting:Tables | 10 |

**Complexity:**
- `H`: 0 (0.0%)
- `M`: 16 (18.6%)
- `L`: 70 (81.4%)

### [DIR] `ppm/`

**Files:** 34 | **Lines of Code:** 4,176

| Step Type | Count |
|-----------|-------|
| DATA Steps | 146 |
| PROC Steps | 275 |
| **PROC Types** | |
|   - PROC APPEND | 1 |
|   - PROC CONTENTS | 1 |
|   - PROC DATASETS | 4 |
|   - PROC EXPORT | 14 |
|   - PROC FREQ | 46 |
|   - PROC IMPORT | 2 |
|   - PROC MEANS | 1 |
|   - PROC PRINT | 1 |
|   - PROC PRINTTO | 2 |
|   - PROC RANK | 1 |
|   - PROC SGPLOT | 39 |
|   - PROC SORT | 123 |
|   - PROC SQL | 32 |
|   - PROC SURVEYSELECT | 2 |
|   - PROC TABULATE | 3 |
|   - PROC TRANSPOSE | 3 |
| Macros Defined | 44 |
| Macro Calls | 42 |

**Step Categories:**

| Category | Count |
|----------|-------|
| Data Management:Transformation | 199 |
| Analyses:Descriptive | 47 |
| Data Management:Join | 40 |
| Reporting:Visualization | 39 |
| Data Management:Query | 32 |
| Integration:File Io | 18 |
| Analyses:Aggregation | 15 |
| Data Management:Filter | 13 |
| Data Management:Generation | 6 |
| Data Management:Metadata | 5 |
| Reporting:Tables | 4 |
| Analyses:Sampling | 2 |
| Analyses:Standardization | 1 |

**Complexity:**
- `H`: 14 (3.3%)
- `M`: 71 (16.9%)
- `L`: 336 (79.8%)
