# hERG Performance Analysis
## Overview
The hERG Performance Analysis repository is a dedicated to validating the various hERG models hosted in the Ersilia model hub. This README provides detailed information on the models included in the validation, the data acquisition process, data pre-processing techniques, and the construction of an unbiased tests dataset for evaluation.

## Models Used in Validation
The following ersilia hERG models were included in the validation process:

eos2ta5 - [Repository Link](https://github.com/ersilia-os/eos2ta5)

eos4tcc - [Repository Link](https://github.com/ersilia-os/eos4tcc)

eos30f3 - [Repository Link](https://github.com/ersilia-os/eos30f3)

eos30gr - [Repository Link](https://github.com/ersilia-os/eos30gr)



## Data Acquisition

### Training dataset
The datasets in the model_datasets folder comprise the training data utilized for each hERG model, sourced directly from the respective model's codebase.

For the eos2ta5 model, the training dataset was retrieved from the source code available [here](https://github.com/Abdulk084/CardioTox/tree/master/data), incorporating files  `external_test_set_neg.csv`, `external_test_set_pos.csv`, `external_test_set_new.csv`, and `train_validation_cardio_tox_data.tar.xz`.

Regarding the eos4tcc model, its training dataset consists of files obtained from the finetuning directory within the source code accessible [here](https://github.com/GIST-CSBL/BayeshERG/tree/main/data/Finetuning).

The eos30f3 model was trained using a dataset introduced by Cai et al. in their work published in J Chem Inf Model, 2019. This dataset comprises 7889 molecules with various cutoffs for hERG blocking activity, available [here](https://github.com/AI-amateur/DMPNN-hERG/blob/main/Figure2/Cai_TableS3_fixed.csv)

Lastly, for the eos30gr model, the training dataset was directly provided within the source code, accessible [here](https://github.com/ChengF-Lab/deephERG/blob/master/Table%20S6.xlsx)

These datasets undergo preprocessing where we clean up the original files and add the InChiKey of the smiles if not available. After preprocessing, each dataset is stored under data/model_datasets/{model_name}_processed.csv. These processed files are then concatenated and duplicates are removed based on Inchikeys to create our training_set.csv

### Test dataset
The test dataset was obtained from the [NCATS repository](https://github.com/ncats/herg-ml/tree/master/data/train_valid). Molecules already present in the training_set.csv were eliminated. This is to ensure fair and unbiased evaluation of the hERG models included in the validation process.


## Repository Structure
## Data folder

- **Combined Training Datasets:** Find the combined training datasets of hERG models in the hub in the 'data' folder, named `combined_dataset_eos.csv`.

- **Unbiased Test Dataset:** Access the unbiased dataset with a 10 μM cut-off in the 'data' folder, named `test_dataset.csv`.

- **model_reproducibility folder:**
      - **model_datasets:** This contains test/validation datasets from the original publication used to test for model reproducibility. eos2ta5_Test-set-I.csv found in the original source code [here](https://github.com/Abdulk084/CardioTox/blob/master/data/external_test_set_pos.csv) is utilized for eos2ta5. [eos4tcc_EX1.csv](https://github.com/GIST-CSBL/BayeshERG/blob/main/data/External/EX1.csv) is the external dataset used to test for the model reproducibility. eos30gr_TableS4.xlsx is the TableS4 file sourced from the supporting information of the publication of eos30gr and eos30gr_validation_set.csv is the validation sheet extracted from it. 
      - **predictions_data:** Predictions are ran on the files in the model_reproducibility/model_datasets and stored here in model_reproducibility/predictions_data in files named reproducibility_predictions_eos2ta5.csv, reproducibility_predictions_eos4tcc.csv qnd reproducibility_predictions_eos30gr.csv


## Notebooks folder

### 1. Data processing
- File: `00_data_processing.ipynb`
- Description: In this notebook we compare model training datasets, get the ratio of activity in each dataset, build the test dataset and perform a PCA analysis between the testing and training data.

### 2. Model Evaluation
- File: `01_model_evaluations.ipynb`
- Description: Assess individual model performance. This notebook provides accuracy, precision, recall, and F1 score, along with an AUROC curve for each model.model performances are compared through scatter plots. This notebook also generates a correlation matrix for a deeper understanding.

### 3. Model Reproducibility
- File: `03_model_reproducibility.ipynb`
- Description: In this notebook, reproducibility experiments are carried out on the hERG models. one specific example from the original publications is reproduced per each model.

#### eos2ta5
 To perform validation for eos2ta5,in page 7 of the original [publication](file:///C:/Users/ENVY%20X360/Downloads/s13321-021-00541-z%20(3).pdf) the authours tested [Test-set-I.csv](https://github.com/Abdulk084/CardioTox/blob/master/data/external_test_set_pos.csv) on their model and got the following results. 
<img src="figures/eos2ta5.png" alt="eos2ta5_publication_result">
When testing eos2ta5 hosted in the hub against that dataset, we got identical results. 
```
MCC: 0.5993902797701955
NPV: 0.6875
ACC: 0.8181818181818182
PPV: 0.8928571428571429
SPE: 0.7857142857142857
SEN: 0.8333333333333334
B-ACC: 0.8095238095238095
```
This model has successfully been reproduced.

#### eos4tcc
The authors tested BayeshERG with the [dataset](https://github.com/GIST-CSBL/BayeshERG/blob/main/data/External/EX1.csv) against other DL models and got the following results.
<img src="figures/eos4tcc.png" alt="eos4tcc_publication_result">
<img src="figures/eos4tcc2.png" alt="eos4tcc_publication_result">

Testing eos4tcc with the same dataset gave the following results.
```
SEN: 0.8333333333333334
SPE: 0.7857142857142857
MCC: 0.5993902797701955
B-ACC: 0.8095238095238095
F1: 0.8620689655172413
```

#### eos30gr
eos30gr was retrained inhouse with a threshold value of 10μM. The author tested the models with different threshold values with the following [validation set](https://github.com/leilayesufu/model-validations/blob/main/hERG/data/model_reproducibility/model_datasets/eos30gr_validation_set.csv) and got the following results using the threshold of 10μM. 

<img src="figures/eos30gr.png" alt="eos30gr_publication_result">

Testing with the eos30gr, the following results were gotten
```
Sensitivity (SE): 0.9977011494252873
Specificity (SP): 0.7857142857142857
Positive Predictive Value (Q+): 1.0
Negative Predictive Value (Q-): 0.6875
Overall Accuracy (Q): 0.9987325728770595
Area Under the Curve (AUC): 1.0
```


