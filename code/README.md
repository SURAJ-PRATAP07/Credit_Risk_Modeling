# Data

The dataset is not stored in this repository (it is >100 MB).

Download it from Kaggle: https://www.kaggle.com/datasets/gabrieloliveirasan/lending-club-credit-risk

```bash
pip install kaggle
# put your kaggle.json in ~/.kaggle/ first (Kaggle -> Settings -> Create New Token)
kaggle datasets download -d gabrieloliveirasan/lending-club-credit-risk -p data --unzip
```

Expected file: `data/loan.csv`
