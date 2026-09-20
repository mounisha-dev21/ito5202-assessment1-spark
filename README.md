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
| `assessment1.ipynb` | Main notebook containing all code and analysis |
| `proposal/proposal.md` | Approved dataset proposal |
| `data/README.md` | Dataset download instructions (data files excluded from repository due to size) |
---

## Analysis overview

Our notebook analyses freight charges across shipping lanes in the Olist marketplace data. Order items are combined with order, customer, seller, product, category translation, as well as geolocation data to create a cached base view of ~110,000 delivered items.

Our analysis then calculates:

- **Shipping lane** based on seller state and customer state
- **Freight charged per kilogram**, which is calculated as total freight divided by total weight for each lane and quarter
- **Rank within quarter**, which compares each lane with other qualifying lanes in the same period
- **Change over time**, which compares each lane with its previous observed quarter

In order to avoid rankings being driven by groups containing only a small number of items, our final results are limited to lane-quarters with at least 30 items. This retains 348 of the 1,856 observed lane-quarter groups while still covering 92.3% of the order items.

---

## Running the notebook

You will first need to activate a virtual environment and start Jupyter by running the following:

```bash
source .venv/bin/activate
jupyter notebook
```

Then open `assessment1.ipynb` and run the notebook from the top using a fresh kernel.

Running the cells in order is important because some DataFrames are reassigned as the analysis progresses, and the benchmarking in Part B depends on the expected cache state.

While Spark is running, its Web UI can usually be accessed at `http://localhost:4040`. If that port is already being used, Spark will move to the next available port. You will be able to see in the environment setup cell what the actual Web UI address is for the current session.

