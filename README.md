# GNSS Signal Classification (LOS / NLOS) using Random Forest and SVM

Machine learning project that classifies GNSS satellite signals as **LOS (Line-of-Sight)** or **NLOS (Non-Line-of-Sight)** using **Random Forest** and **Support Vector Machine (SVM)**.

## Why this project?

In cities, GNSS signals bounce off buildings before reaching the receiver. These reflected (NLOS) signals cause position errors. If we can detect NLOS signals, we can remove or down-weight them and get a better position.

## Dataset

**Data source:** The dataset was provided by IIT Tirupati as part of the STAR PNT project.

File: `gnss_data.xlsx` (sheet: `Sheet1`)

| Column | Meaning |
| --- | --- |
| Year, Month, Date | Date of the observation (2 to 6 June 2023, plus 1 June) |
| Hour, Min, Sec | Time of the observation (readings about 2 seconds apart) |
| PRN | Satellite ID with system name, e.g. `GPS/ 3` or `GPS/ 8` |
| Elevation | Satellite angle above the horizon (degrees) |
| Azimuth | Satellite direction (degrees) |
| SNR | Signal-to-noise ratio of the received signal (dB-Hz) |
| Label | Target class: `LOS` or `NLOS` |

**Size:** about 133,500 rows and 11 columns. Two satellites are present: GPS 3 and GPS 8.

**Class balance:** roughly 51% LOS and 49% NLOS, so the classes are fairly balanced.

### Data quality notes

- The first row of the sheet is blank.
- About 13,000 rows have empty feature values (some of them still carry a label). These rows are dropped before training.
- `PRN` contains extra spaces (`GPS/  8`), so it should be cleaned before use.

## Method

1. **Load** the Excel file with pandas.
2. **Clean:** drop blank and incomplete rows, tidy the `PRN` text.
3. **Features:** `Elevation`, `Azimuth`, `SNR` (optionally the satellite ID).
4. **Target:** `Label` converted to numbers (LOS = 0, NLOS = 1).
5. **Split** into training and testing sets.
6. **Scale** features with `StandardScaler` (needed for SVM; not needed for Random Forest).
7. **Train** Random Forest and SVM.
8. **Evaluate** with accuracy, precision, recall, F1-score and a confusion matrix.

## Models

| Model | Idea in one line | Notes |
| --- | --- | --- |
| Random Forest | Many decision trees vote together | Fast, handles non-linear data, gives feature importance |
| SVM (RBF kernel) | Finds the best boundary between the two classes | Needs scaled features, slow on very large data |

## Results

Fill in your own numbers after running the notebook.

| Model | Accuracy | Precision | Recall | F1-score |
| --- | --- | --- | --- | --- |
| Random Forest | | | | |
| SVM | | | | |

Add your confusion matrix and feature importance plot images here:

```
![Confusion matrix](images/confusion_matrix.png)
![Feature importance](images/feature_importance.png)
```

## Project structure

```
gnss-los-nlos-classification/
├── data/
│   └── gnss_data.xlsx
├── notebooks/
│   └── gnss_rf_svm.ipynb
├── images/
│   └── (result plots)
├── requirements.txt
├── .gitignore
└── README.md
```

## How to run

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/gnss-los-nlos-classification.git
cd gnss-los-nlos-classification

# 2. Install the libraries
pip install -r requirements.txt

# 3. Open the notebook
jupyter notebook notebooks/gnss_rf_svm.ipynb
```

`requirements.txt`

```
pandas
numpy
openpyxl
scikit-learn
matplotlib
seaborn
jupyter
```

## Tips and things to watch

- **Avoid inflated accuracy.** Readings are only 2 seconds apart, so neighbouring rows look almost identical. A purely random train/test split lets the model "see" near-copies of test rows during training. For a fairer test, split by time (for example train on 1 to 5 June, test on 6 June) or by satellite.
- **SVM speed.** With over 100,000 rows, an RBF SVM can take a long time. Train it on a sample, or try `LinearSVC`.
- **Scaling.** Always fit the scaler on the training set only, then apply it to the test set.
- **Reproducibility.** Use `random_state=42` in the split and the models.

## Future work

- Try more models (XGBoost, KNN, neural network).
- Add more satellites and more days of data.
- Add features such as elevation change over time or SNR change over time.
- Tune hyperparameters with `GridSearchCV`.

## Author

Dr. Alekhya Lanka · [GitHub](https://github.com/your-username)

## License

This project is released under the data provided by IIT Thirupathi for PNT projects 

