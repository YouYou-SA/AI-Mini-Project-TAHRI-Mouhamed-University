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
From Anaconda Navigator
