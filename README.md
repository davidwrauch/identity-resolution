# Identity Graph & Entity Resolution Pipeline

## Overview

This project demonstrates a production-style identity resolution workflow inspired by challenges commonly encountered in political data, CRM systems, voter files, customer databases, and marketing platforms.

Starting with the FEBRL benchmark dataset, the pipeline performs:

- Multi-pass blocking to generate candidate record pairs
- Fuzzy matching using Jaro-Winkler similarity
- Probabilistic record linkage
- Model-based duplicate detection
- Entity resolution through graph clustering
- Construction of a multi-attribute identity graph

The project compares traditional rules-based matching against machine learning approaches and evaluates how different blocking strategies impact downstream matching performance.

---

## Why This Project?

Organizations often receive records from multiple vendors, data providers, and operational systems.

The same person may appear with:

- Different name spellings
- Missing identifiers
- Multiple phone formats
- Different email addresses
- Incomplete records

Identity resolution attempts to determine which records belong to the same real-world individual.

This project simulates that process and demonstrates how graph-based approaches can be used to unify fragmented identities.

---

## Dataset

The project uses the FEBRL (Freely Extensible Biomedical Record Linkage) benchmark dataset included in the `RecordLinkage` R package.

The original dataset contains:

- First name
- Last name
- Date of birth
- Ground-truth duplicate labels

To better resemble real-world identity systems, the project generates synthetic:

- Email addresses
- Phone numbers
- Physical addresses
- ZIP codes

Random formatting differences, missing values, and typographical errors are introduced to create a more realistic matching problem.

---

## Pipeline

### 1. Candidate Generation (Blocking)

Rather than comparing every record to every other record, multiple blocking strategies generate candidate pairs:

- Last initial + birth year
- First initial + last initial
- Birth year + month
- Exact email
- Exact phone
- ZIP + last initial
- ZIP + birth year

This dramatically reduces the search space while maintaining high duplicate coverage.

---

### 2. Feature Engineering

For each candidate pair, similarity features are created:

- Jaro-Winkler first-name similarity
- Jaro-Winkler last-name similarity
- Address similarity
- Birth-date agreement indicators
- ZIP agreement
- Email agreement
- Phone agreement
- Number of blocking rules that identified the pair

---

### 3. Model Comparison

The project evaluates four matching approaches:

| Model | Purpose |
|---------|---------|
| Rules-Based | Transparent baseline |
| Logistic Regression | Probabilistic benchmark |
| Random Forest | Nonlinear ensemble |
| XGBoost | Gradient boosting model |

Thresholds are optimized on the training set and evaluated on a held-out test set.

---

### 4. Entity Resolution

Predicted matches are converted into graph edges.

Records become graph nodes:

```text
Record A ---- Record B
```

Connected components are then resolved into unified entities:

```text
Record A
      \
       Entity 12
      /
Record B
```

This mirrors identity resolution approaches used in customer data platforms, voter data systems, and commercial identity graphs.

---

### 5. Attribute Identity Graph

A second graph is constructed using multiple node types:

```text
Entity
 ├── Record
 ├── Email
 ├── Phone
 └── Address
```

Example:

```text
Entity_42
    |
Record_117
    |
john.smith@gmail.com

Entity_42
    |
Record_117
    |
555-123-4567
```

This structure more closely resembles production identity graphs used by organizations such as Acxiom, Experian, Civis Analytics, BlueLabs, and political data vendors.

---

## Example Results

Using the FEBRL benchmark dataset:

- 100% blocking recall
- Perfect recovery of known duplicate pairs on held-out test data
- Multi-model comparison framework
- Record-level identity graph
- Attribute-level identity graph

Performance will vary as additional noise and matching complexity are introduced.

---

## Outputs

The pipeline exports:

```text
output/
├── enriched_records.csv
├── candidate_pairs.csv
├── scored_pairs.csv
├── model_comparison_metrics.csv
├── blocking_recall.csv
├── rule_performance.csv
├── resolved_entities.csv
├── record_identity_graph_edges.csv
├── attribute_identity_graph_nodes.csv
└── attribute_identity_graph_edges.csv
```

---

## Skills Demonstrated

- Record Linkage
- Entity Resolution
- Identity Graphs
- Fuzzy Matching
- Graph Analytics
- Feature Engineering
- Logistic Regression
- Random Forests
- XGBoost
- Network Analysis
- Data Quality Assessment
- Machine Learning Evaluation
- R Programming

---

## Future Improvements

- Fellegi-Sunter probabilistic linkage
- Active learning for uncertain matches
- Graph embeddings
- Community detection algorithms
- Bayesian entity resolution
- Multi-source identity resolution
- Incremental graph updates
- Spark-scale implementation

---

## Technologies

- R
- tidyverse
- RecordLinkage
- stringdist
- igraph
- ranger
- xgboost
- broom

---

## Author

David Rauch

Data Scientist focused on experimentation, causal inference, decision systems, and graph-based analytics.
