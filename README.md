# What Drives the Price of a Car?

**Practical Application 11.1 — CRISP-DM Used Car Price Analysis**

This project explores a Kaggle dataset of ~426K used-car listings from Craigslist to determine which vehicle attributes most strongly influence price. The findings are translated into actionable inventory recommendations for a used-car dealership.

## 📓 Notebook

The full analysis lives in **[`prompt_II.ipynb`](prompt_II.ipynb)**.

## 🗂 Repository Structure

```
.
├── README.md             # this file
├── prompt_II.ipynb       # full CRISP-DM analysis
└── data/
    └── vehicles.csv      # raw dataset (download separately, see below)
```

## 🚀 How to Reproduce

1. Clone this repository.
2. Download the dataset from the assignment link and place `vehicles.csv` in the `data/` folder.
3. Install dependencies:
   ```bash
   pip install pandas numpy scikit-learn matplotlib seaborn jupyter
   ```
4. Launch the notebook:
   ```bash
   jupyter notebook prompt_II.ipynb
   ```
5. Run all cells.

## 🧭 CRISP-DM Steps Followed

1. **Business Understanding** — translate the dealer's question into a regression problem with RMSE as the success metric.
2. **Data Understanding** — profile the 426K-row dataset, examine missingness, distribution of price, and cardinality of categoricals.
3. **Data Preparation** — drop identifiers, filter implausible prices/mileages/years, engineer `car_age`, impute missing values, and one-hot encode categoricals (with rare-category bucketing).
4. **Modeling** — fit and tune three regression models (Linear, Ridge, Lasso) with 5-fold cross-validation and `GridSearchCV` over the regularisation strength.
5. **Evaluation** — compare models on test-set RMSE, MAE, and R²; interpret the surviving Ridge coefficients to identify price drivers.
6. **Deployment** — translate model output into plain-English recommendations for the dealership.

## 🔑 Key Findings

After cleaning the data and fitting three regression models, the dominant patterns in used-car pricing are:

- **Age and mileage are the two biggest negative drivers of price.** Each additional year of age and each additional 10K miles takes a measurable, consistent bite out of resale value.
- **Condition only really matters at the top.** *New* and *like-new* vehicles command a clear premium; the gap between *good*, *excellent*, and *fair* is comparatively small.
- **Powertrain configuration carries a premium.** Diesel engines, 4-wheel drive, and pickup/truck body types consistently outprice their gasoline / 2wd / sedan counterparts even after controlling for age and mileage.
- **Brand matters.** Toyota, Ford, RAM, and GMC hold value notably better than budget brands.
- **Many listing fields add little signal.** Lasso pruned a large share of the categorical features — paint colour and several minor fields don't meaningfully move price.

## 💡 Recommendations to the Dealership

| Recommendation | Why |
|---|---|
| Prioritise low-age, low-mileage inventory. | Age and odometer are the strongest negative coefficients in every model. |
| Stock diesel pickups and 4wd SUVs. | They consistently price above sedans of similar age. |
| Don't over-pay for "good" vs "excellent" condition. | The price tier collapses below *like new*. |
| Lean toward Toyota, Ford, RAM, GMC. | Top positive manufacturer coefficients. |
| Don't worry about paint colour. | Negligible impact once other features are controlled for. |

## ⚠️ Caveats

- The data are **listing prices**, not sale prices — actual transactions likely run ~5–10% lower.
- Geographic features (state/region) were dropped in this iteration; bringing them back via target encoding would capture local market effects.
- Listing quality (photos, description text) is invisible to the model.

## 🔭 Next Steps

1. Try non-linear models (Random Forest, Gradient Boosting) — typically 15–25% RMSE improvement on mixed-type tabular data.
2. Re-introduce geography via target encoding.
3. Retrain on actual sale prices if available (instead of listings).
4. Wrap the final model in a small internal pricing tool for buyers at auction.
