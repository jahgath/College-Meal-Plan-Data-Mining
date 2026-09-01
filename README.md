# Can Data Science Build a Nutritious College Meal Plan for Under $50/Week?

A data-mining experiment combining food-price data, nutritional databases,
deep-learning-based entity matching, recipe preference modeling, and
mathematical optimization to investigate whether a practical and nutritious
college meal plan can be constructed for under $50 per week.

## Final Result

The final optimization produced a nutritionally constrained basket with:

- Estimated weekly cost: **$20.60**
- **18 distinct USDA food categories**
- Approximately **1,900 kcal/day**
- Approximately **100 g protein/day**
- Approximately **43.6 g fiber/day**
- Modeled calcium, iron, potassium, vitamin C, and vitamin D requirements satisfied
- Sodium and saturated fat below modeled upper limits

> Price estimates are based on USDA 2017–2018 national-average data and should
> not be interpreted as current grocery-store prices.

## Project Overview

The project began by attempting to combine Walmart grocery prices with
Open Food Facts nutrition data.

### Experiment 1 — Product Matching

Exact product-name matching produced only **1 match**.

After text normalization, this increased to **1,131 matches**.

A pretrained Sentence Transformer (`all-MiniLM-L6-v2`) was then used to
perform semantic product matching.

This produced:

- 20,376 candidate products
- 6,894 high-confidence hybrid matches

However, validation revealed that semantic similarity did not guarantee
nutritional equivalence.

For example, a Walmart black-bean product was semantically matched with an
Open Food Facts black-bean record that appeared to represent a different food
form.

This led to an important finding:

> **Semantic similarity does not imply nutritional equivalence.**

The Walmart/Open Food Facts integration was therefore rejected for the final
nutrition calculations.

## USDA Data Pivot

The project switched to standardized USDA datasets:

- USDA Purchase to Plate National Average Prices (PP-NAP)
- USDA Food and Nutrient Database for Dietary Studies (FNDDS) 2017–2018

Both datasets contain standardized food identifiers.

Joining them using `food_code` resulted in:

**4,435 / 4,435 price records successfully matched with nutrition data.**

This provided a reliable dataset containing both price and nutritional
information per 100 grams.

## Optimization Experiments

### Pure Nutrition Optimization

Objective:

> Minimize weekly cost while satisfying nutritional constraints.

Result:

**$8.24/week**

The optimizer selected only five foods, including large quantities of split
peas, dry milk powder, corn oil, bran cereal, and fortified drink powder.

The solution was mathematically valid but practically unrealistic.

### Food-Group Optimization

Food-group and quantity constraints were introduced.

Result:

**$19.89/week**

The diet became more balanced but exploited multiple USDA records representing
similar foods.

This demonstrated:

> **Database-record diversity is not dietary diversity.**

### Preference-Aware Experiment

Food.com user ratings were analyzed using a Bayesian weighted preference score.

The dataset was filtered for recipes that were:

- sufficiently reviewed
- inexpensive
- suitable as main dishes/lunches
- <=60 minutes
- 3–15 ingredients

This produced **481 inexpensive, popular, college-friendly recipe candidates**.

Directly maximizing ingredient popularity caused another optimization loophole,
so recipe preference was separated from nutrition optimization.

### Genuine-Variety Optimization

The final model used category-level binary variables and nutritional constraints
to require genuine food diversity.

Result:

**$20.60/week**

with **18 distinct USDA food categories**.

## Recipe Preference Layer

Food.com ratings were used to identify recognizable meal structures compatible
with the optimized ingredient basket.

Examples included:

- Creamy chickpea curry
- Vegetable fried rice
- Bean and corn bowls
- Peanut-soy noodles
- Peanut-banana breakfasts

USDA remains the source of truth for nutrition and price. Food.com is used only
as a preference and meal-compatibility signal.

## Key Lessons

1. Better models cannot always rescue incompatible datasets.
2. Validation matters more than impressive-looking matching metrics.
3. Standardized identifiers can outperform sophisticated semantic matching.
4. Optimization algorithms aggressively exploit modeling assumptions.
5. Nutritional adequacy is much easier to model than human food preference.

## Technologies

- Python
- Pandas
- NumPy
- Matplotlib
- PuLP / Mixed-Integer Linear Programming
- Sentence Transformers
- `all-MiniLM-L6-v2`
- RapidFuzz
- KaggleHub
- USDA PP-NAP
- USDA FNDDS
- Food.com Recipes & User Interactions

## Repository Structure

```text
notebooks/    Complete analysis notebook
figures/      Figures used in the analysis/article
data/         Dataset acquisition instructions