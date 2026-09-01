# Datasets

This project uses several publicly available datasets for grocery prices,
nutrition information, product matching, and recipe preference analysis.

Raw datasets are **not included in this repository** due to their size and
because they are available from their original sources.

The notebook contains the complete data-processing and analysis workflow.

---

## 1. Open Food Facts — World Food Facts

**Source:** Kaggle  
**Dataset:** World Food Facts

https://www.kaggle.com/openfoodfacts/world-food-facts

Open Food Facts is a collaborative database containing nutritional information
for hundreds of thousands of food products.

### Used for

- Initial nutritional dataset
- Product-name normalization
- Walmart ↔ Open Food Facts entity matching
- Deep-learning semantic matching experiment

This dataset was ultimately **not used for the final nutrition calculations**
because semantic product similarity did not always imply nutritional
equivalence.

---

## 2. Walmart Grocery Product Dataset

**Source:** Kaggle  
**Dataset:** Walmart Grocery Product Dataset

https://www.kaggle.com/datasets/polartech/walmart-grocery-product-dataset

The dataset contains Walmart grocery product information collected across
multiple shipping locations.

Important fields used in this project include:

- Product name
- Brand
- Category
- Product size
- Retail price
- Current price
- Shipping location

### Used for

- Grocery-price exploration
- Product normalization
- Price-distribution analysis
- Deep-learning Walmart ↔ Open Food Facts matching

The dataset contained approximately 568,000 observations representing roughly
30,000 unique products.

---

## 3. USDA Purchase to Plate — National Average Prices (PP-NAP)

**Source:** U.S. Department of Agriculture Economic Research Service

https://www.ers.usda.gov/data-products/purchase-to-plate

PP-NAP provides standardized national-average food price estimates.

The project uses the **2017–2018** price cycle.

Important fields include:

- `food_code`
- `food_description`
- `price_100gm`

### Used for

- Final food-price calculations
- Price-per-100g standardization
- USDA price/nutrition integration
- Meal-plan optimization

---

## 4. USDA Food and Nutrient Database for Dietary Studies (FNDDS) 2017–2018

**Source:** USDA Agricultural Research Service

https://www.ars.usda.gov/northeast-area/beltsville-md-bhnrc/beltsville-human-nutrition-research-center/food-surveys-research-group/docs/fndds-download-databases/

The **FNDDS 2017–2018 Nutrient Values** workbook provides nutrient information
for thousands of standardized foods.

Important variables used include:

- Energy
- Protein
- Carbohydrates
- Dietary fiber
- Total fat
- Saturated fat
- Sodium
- Potassium
- Calcium
- Iron
- Vitamin C
- Vitamin D

### Used for

- Final nutrition calculations
- Nutritional constraints
- USDA optimization model

The FNDDS data was joined with PP-NAP using the standardized USDA `food_code`.

This produced a **100% match rate for the 4,435 PP-NAP 2017–2018 price
records used in the project**.

---

## 5. Food.com Recipes and User Interactions

**Source:** Kaggle  
**Dataset:** Food.com Recipes and User Interactions

https://www.kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions

Files used:

- `RAW_recipes.csv`
- `RAW_interactions.csv`

The dataset contains recipe information and user ratings/reviews.

### Used for

- Recipe popularity analysis
- Bayesian weighted preference scoring
- College-friendly recipe filtering
- Meal-preparation practicality analysis
- Ingredient overlap with the optimized USDA food basket

Food.com was used as a **preference and meal-compatibility layer only**.

Nutrition and price calculations remained based on USDA data.

---

# Data Pipeline

The project evolved through two major data strategies:

```text
Walmart Prices
      +
Open Food Facts Nutrition
      |
      v
Deep-Learning Product Matching
      |
      v
Semantic equivalence problem
      |
      X
Method rejected for final nutrition calculations


USDA PP-NAP Prices
      +
USDA FNDDS Nutrition
      |
      v
Exact food_code Join
      |
      v
Nutrition + Price Optimization
      |
      +
Food.com Recipe Preferences
      |
      v
College Meal-Prep Plan