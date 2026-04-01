# AI-Mini-Project-TAHRI-Mouhamed-University
Optimal resource allocation in precision agriculture (multi-output regression: predict optimal water/fertilizer allocation) Synthetic agricultural allocation dataset


Launch Guide — Precision Agriculture AI Project
Step 1 — Project Folder Setup
Before opening anything, organise your folder exactly like this:
mini project AI/          your working directory
 agriculture_dataset_generator.ipynb
 agriculture_cleaning.ipynb
 agriculture_feature_engineering.ipynb
 agriculture_splitting.ipynb
 agriculture_training.ipynb
 agriculture_evaluation.ipynb
 
 datasets/                  created automatically
 splits/                    created automatically
 models/                    created automatically
 All 6 notebooks must be in the same folder. The folders datasets/, splits/, and models/ are created automatically when you run the notebooks  don't create them manually.
 
 Step 2  Install Required Libraries
Open Anaconda Prompt (not the regular terminal) and run these commands one by one:
pip install numpy
pip install pandas
pip install matplotlib
pip install seaborn
pip install scikit-learn
pip install jupyterlab
Or install everything in one command:
pip install numpy pandas matplotlib seaborn scikit-learn jupyterlab
To verify the installation worked:
python -c "import numpy, pandas, matplotlib, seaborn, sklearn; print('All OK')"
You should see: All OK

Step 3 — Launch Jupyter Notebook
Option A — From Anaconda Navigator:

cd "C:\path\to\mini project AI"
jupyter notebook
```
Replace `C:\path\to\mini project AI` with your actual folder path.

---

## Step 4 — Run Notebooks IN ORDER

> ⚠️ **This is the most important rule.** Each notebook saves files that the next one reads. Running them out of order will cause errors.

---

### Notebook 1 — `agriculture_dataset_generator.ipynb`

**What it does:** Generates both datasets from scratch.

**How to run:**
1. Open the file in Jupyter
2. Click **Kernel → Restart & Run All**
3. Wait until all cells finish (a number appears in `[  ]` for each cell)

**Files it creates:**
```
datasets/dataset1_agriculture_raw.csv   ← 2040 rows, raw with noise
datasets/dataset2_agriculture_genAI.csv ← 500 rows, GenAI version
```

**Expected output in the last cell:**
```
Dataset 1 saved to: datasets/dataset1_agriculture_raw.csv
Shape  : (2040, 15)
```

---

### Notebook 2 — `agriculture_cleaning.ipynb`

**What it does:** Cleans Dataset 1 (removes duplicates, imputes missing values, removes outliers, encodes and scales).

**Requires:** `datasets/dataset1_agriculture_raw.csv` ← must exist first

**How to run:** Click **Kernel → Restart & Run All**

**Files it creates:**
```
datasets/dataset1_clean.csv        ← unscaled, encoded (1989 rows)
datasets/dataset1_clean_std.csv    ← Z-score scaled
datasets/dataset1_clean_mm.csv     ← Min-Max scaled
```

**Expected output in the last cell:**
```
Saved:
  datasets/dataset1_clean.csv
  datasets/dataset1_clean_std.csv
  datasets/dataset1_clean_mm.csv
```

---

### Notebook 3 — `agriculture_feature_engineering.ipynb`

**What it does:** PCA, LDA, feature selection — produces reduced datasets.

**Requires:** `datasets/dataset1_clean_std.csv` ← must exist first

**How to run:** Click **Kernel → Restart & Run All**

> ⚠️ This notebook takes **30–60 seconds** longer than the others. The Random Forest fit for feature importance is the slow part. This is normal — wait for it.

**Files it creates:**
```
datasets/dataset1_pca.csv       ← PCA projected (15 components)
datasets/dataset1_selected.csv  ← 6 best features only
datasets/dataset1_lda.csv       ← LDA projected
```

**Expected output in the last cell:**
```
PCA (95% variance threshold)
  → 15 components
  → Variance retained: 96.67%
```

---

### Notebook 4 — `agriculture_splitting.ipynb`

**What it does:** Splits Dataset 1 into Train/Val/Test and runs K-Fold CV on Dataset 2.

**Requires:**
- `datasets/dataset1_clean_std.csv`
- `datasets/dataset2_agriculture_genAI.csv`

**How to run:** Click **Kernel → Restart & Run All**

**Files it creates:**
```
splits/split_data.pkl    ← all split arrays saved as binary file
```

**Expected output:**
```
Split summary (Dataset 1):
  Training   :  1392 rows  (70.0%)
  Validation :   298 rows  (15.0%)
  Test       :   299 rows  (15.0%)

Mean val R² : 0.3912  ± 0.0205
```

---

### Notebook 5 — `agriculture_training.ipynb`

**What it does:** Trains all 3 models (RandomForest, SVR, MLP) and compares them.

**Requires:** `splits/split_data.pkl` ← must exist first

**How to run:** Click **Kernel → Restart & Run All**

> ⚠️ **This notebook is the slowest.** Expected times on a standard laptop:
> - RandomForest: ~10–20 seconds
> - SVR: ~30–60 seconds
> - MLP: ~1–3 minutes (with early stopping)
>
> **Do not interrupt it.** The progress bar will appear for SVR.

**Files it creates:**
```
models/rf_model.pkl      ← trained RandomForest
models/svr_model.pkl     ← trained SVR
models/mlp_model.pkl     ← trained MLP
models/results.pkl       ← comparison metrics
```

**Expected output:**
```
RandomForest — Val R²=0.6109  Test R²=0.6329
SVR          — Val R²=0.6667  Test R²=0.7217
MLP          — Val R²=0.6842  Test R²=0.7476
Best model by Test R² : MLP
```

---

### Notebook 6 — `agriculture_evaluation.ipynb`

**What it does:** Full evaluation, residual analysis, and GridSearchCV tuning.

**Requires:**
- `splits/split_data.pkl`
- `models/rf_model.pkl`
- `models/svr_model.pkl`
- `models/mlp_model.pkl`

**How to run:** Click **Kernel → Restart & Run All**

> ⚠️ **GridSearchCV is the slowest part of the entire project** (Cell 12). It runs 405 training operations. On a standard laptop this takes **3–10 minutes**. You will see this in the output:
> ```
> Fitting 5 folds for each of 81 candidates, totalling 405 fits
> ```
> This is completely normal. Do not restart the kernel.

**Files it creates:**
```
models/rf_tuned_model.pkl        ← best tuned model
datasets/final_predictions.csv   ← test set predictions vs actuals
```

**Expected final output:**
```
========================================
  PROJECT PART 1 — COMPLETE
========================================
  Final Test R²  : 0.7022
  Final RMSE     : 33.824
  Best params    : {'max_depth': None, 'max_features': 0.5, ...}
```

---

## Step 5 — Summary: Correct Execution Order
```
① dataset_generator   →  creates  datasets/dataset1_agriculture_raw.csv
                                   datasets/dataset2_agriculture_genAI.csv
         ↓
② cleaning            →  reads    datasets/dataset1_agriculture_raw.csv
                          creates  datasets/dataset1_clean_std.csv  (+ 2 others)
         ↓
③ feature_engineering →  reads    datasets/dataset1_clean_std.csv
                          creates  datasets/dataset1_pca.csv  (+ 2 others)
         ↓
④ splitting           →  reads    datasets/dataset1_clean_std.csv
                                   datasets/dataset2_agriculture_genAI.csv
                          creates  splits/split_data.pkl
         ↓
⑤ training            →  reads    splits/split_data.pkl
                          creates  models/rf_model.pkl
                                   models/svr_model.pkl
                                   models/mlp_model.pkl
         ↓
⑥ evaluation          →  reads    splits/split_data.pkl
                                   models/*.pkl
                          creates  models/rf_tuned_model.pkl
                                   datasets/final_predictions.csv
```

---

## Common Errors and Fixes

| Error message | Cause | Fix |
|---|---|---|
| `FileNotFoundError: datasets/dataset1_agriculture_raw.csv` | Notebook 1 not run yet | Run notebook 1 first |
| `FileNotFoundError: splits/split_data.pkl` | Notebook 4 not run yet | Run notebooks 1→4 in order |
| `FileNotFoundError: models/rf_model.pkl` | Notebook 5 not run yet | Run notebooks 1→5 in order |
| `ModuleNotFoundError: No module named 'sklearn'` | sklearn not installed | Run `pip install scikit-learn` |
| `ModuleNotFoundError: No module named 'seaborn'` | seaborn not installed | Run `pip install seaborn` |
| `KeyError: 'water_allocation_L_ha'` | Wrong CSV file loaded | Check you are in the correct folder |
| Kernel dies during GridSearchCV | Not enough RAM | Close all other programs, restart kernel |
| Graphs not showing | Matplotlib backend issue | Add `%matplotlib inline` at top of cell |

---

## Quick Checklist Before Presenting

Before your oral presentation, verify that these files all exist in your project folder:
```
✅ datasets/dataset1_agriculture_raw.csv
✅ datasets/dataset1_clean.csv
✅ datasets/dataset1_clean_std.csv
✅ datasets/dataset2_agriculture_genAI.csv
✅ datasets/dataset1_pca.csv
✅ datasets/dataset1_selected.csv
✅ datasets/final_predictions.csv
✅ splits/split_data.pkl
✅ models/rf_model.pkl
✅ models/svr_model.pkl
✅ models/mlp_model.pkl
✅ models/rf_tuned_model.pkl
✅ All 6 .ipynb notebooks have outputs visible (not empty cells)
If all items are checked, your project is complete and ready to execute live during the presentation.
