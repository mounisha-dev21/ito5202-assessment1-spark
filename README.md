# Assessment 1: Analysing historical data with system performance - Phase 2

**STUDENT ID:** 35721588
**UNIT:** ITO5202
**TEACHING PERIOD:** TP5, 2026

---

## Project Overview

This project involves designing and implementing an analytical query of a large e-commerce dataset using Spark. It also involves thorough analysis into its distributed execution behaviours. There are two key parts to the project:

- ** Part A:** The analytical query is implemented two times; the first using the Spark DataFrame API, and the second with Spark SQL. We also want to conduct some validation of both outputs to ensure equivalent results.
- ** Part B:** We then want to conduct performance analysis of a partitioning strategy, benchmark execution time, and conduct DAG analysis via the Spark Web UI.

---

## Dataset

**Dataset Name:** Brazilian E-Commerce Public Dataset by Olist  
**Dataset Source:** https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce  
**Licence:** CC BY-NC-SA 4.0  
**Data Coverage:** Orders placed on the Olist marketplace between October 2016 and September 2018

The dataset comprises of 9 inter-related .csv files, which is total contain approximately 1.55 million records. 

Note: the dataset files are not committed to this repository due to their size. See `data/README.md` for further information and for dataset download instructions.

---

## Reproducing the environment

This project was developed and executed in Spark local mode on macOS.

**1. Java 17.** Spark 3.5 requires Java 8, 11 or 17 and does not support newer versions reliably. Thus, we installed the JDK 17 package for our platform from https://adoptium.net/temurin/releases/, then we ran the following:

```bash
echo 'export JAVA_HOME=$(/usr/libexec/java_home -v 17)' >> ~/.zshrc
source ~/.zshrc
```

**2. Python 3.11.9.** This was installed from https://www.python.org/downloads/release/python-3119/

**3. Clone and install dependencies.**

The following commands were run to install all dependencies we needed for the project.

```bash
git clone https://github.com/mounisha-dev21/ito5202-assessment1-spark.git
cd ito5202-assessment1-spark
/usr/local/bin/python3.11 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```
---

## Repository contents

| Path | Purpose |
|---|---|
| `README.md` | Project overview and setup instructions |
| `requirements.txt` | Pinned Python dependencies |
| `proposal/proposal.md` | Approved dataset proposal (hurdle requirement) |

---
