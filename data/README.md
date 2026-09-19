# Dataset

For our project, we make use of nine CSV files from the Olist Brazilian E-Commerce dataset. These files were intentionally excluded from our repository because of their size.

* **Dataset:** Olist Brazilian E-Commerce Public Dataset
* **Source:** https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce
* **Licence:** CC BY-NC-SA 4.0
* **Uncompressed size:** ~125MB

## Download and setup

### Pre-requisite

Our chosen dataset is hosted on Kaggle, and downloading datasets from this source require having an authenticated account. If you do not have one, this will need to be done first as per the below steps:

1. Go to https://www.kaggle.com and select **Register**.
2. Sign up using an email address or a linked Google account.
3. Confirm your account via the verification email Kaggle sends.
4. Sign in.

### Dataset download

The dataset must be downloaded separately from Kaggle before running `assessment1.ipynb`.

1. Log in to Kaggle and navigate to the dataset using the link above.
2. Use the **Download** option and choose **Download dataset as zip**.
3. Unzip the downloaded archive directly into the repository's `data/` folder.
4. Keep all CSV files directly inside `data/` and retain their original Kaggle filenames.

For example:

```bash
unzip ~/Downloads/archive.zip -d data/
```

After extraction, the notebook expects the CSV files to be located directly under `data/`, rather than inside any additional folders.

## Confirming the dataset

To check that all required files have been extracted, run:

```bash
ls -1 data/*.csv | wc -l
```

A correctly prepared `data/` directory should return:

```text
9
```

The expected files and their dimensions are listed below:

| Filename                                | Number of rows | Number of columns |
| --------------------------------------- | -------------: | ----------------: |
| `olist_geolocation_dataset.csv`         |      1,000,163 |                 5 |
| `olist_order_items_dataset.csv`         |        112,650 |                 7 |
| `olist_order_payments_dataset.csv`      |        103,886 |                 5 |
| `olist_orders_dataset.csv`              |         99,441 |                 8 |
| `olist_customers_dataset.csv`           |         99,441 |                 5 |
| `olist_order_reviews_dataset.csv`       |         88,224 |                 7 |
| `olist_products_dataset.csv`            |         32,951 |                 9 |
| `olist_sellers_dataset.csv`             |          3,095 |                 4 |
| `product_category_name_translation.csv` |             71 |                 2 |

Once these files are present, assessment1.ipynb can access them using paths relative to the repository root. The original filenames supplied by Kaggle therefore need to remain unchanged.
