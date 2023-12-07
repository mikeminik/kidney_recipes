# Predicting Recipe Suitability for Individuals Managing Chronic Kidney Disease (CKD)

Chronic kidney disease (CKD) affects >10% of the general population and has shown an increase in associated deaths in the last 20 years. Nutritional intake is an important aspect of managing progression of the disease, but identifying the appropriateness of recipe ingredients can be challenging for individuals and time consuming for professionals.

This project sought to answer the question of whether recipe ingredients could be distinguished by their similarity or dissimilarity to existing curated renal-friendly recipe collections using natural language processing (NLP) and classification models.

## Disclaimer:

The suitability of recipes for those managing CKD is based heavily on two understandings:
- That recipes sourced from kidney health-oriented organizations are indeed appropriate for those managing CKD
- That recipes in this project deemed inappropriate for kidney health used nutritional thresholds based on guidelines from online sources (sources available) and are in no way meant to be accurate at a medical level.

Those managing Chronic Kidney Disease should always consult their doctor or registered dietitian. 


# Data Dictionary

For datasets originating from Food.com - nutrition information used was confirmed to be in the following units via inspection of corresponding recipes on the website.
| Nutrition Fact | Unit |
|---|---|
| Sodium | mg |
| Protein | g |
| Fat | g |


|File Name| Origin | Description | 
|---|---|---|
| recipes.csv | [Kaggle: Food.com Recipes and Interactions](https://www.kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions) | Extensive general list of recipes and ingredients |
| recipes_w_ing_qtys.csv | [Kaggle: Food.com - Recipes and Reviews](https://www.kaggle.com/datasets/irkaal/foodcom-recipes-and-reviews)  | Extensive general list of recipes with nutritional content  |
| kidney-kitchen-recipes-with-ingredients.csv | [American Kidney Fund](https://kitchen.kidneyfund.org/find-recipes/) | Curated kidney friendly recipe ingredients and nutrition from the American Kidney Fund
| kidney-ca-recipes-with-ingredients-and-nutrition.csv | [Kidney Foundation of Canada](https://www.kidneycommunitykitchen.ca/kkcookbook/recipes/) | Curated kidney friendly recipe ingredients and nutrition from Kidney Foundation of Canada 
| non_kidney_friendly_recipes.csv | generated | Combination of the Kaggle dataset recipes (joined by recipe Id) with nutritional outliers removed high sodium, fat, and protein flags |
| nkf_sample_10000.csv | generated | A sample of non kidney-friendly recipes with ingredients standardized
| kidney-friendly-standardized.csv | generated | Non augmented kidney-friendly recipes with ingredients standardized
| tidy-recipe-data-all.csv | generated | Labeled, standardized recipe ingredients. Kidney friendly recipes have been augmented. Model-ready.

## Note on data

Those wishing to jump straight to model tuning can simply use tidy-recipe-data-all.csv in combination with the file generation notebook to generation local text files for RNNs. Other files included for thoroughness, as opportunities to enhance standardization and augmentation are certainly there.

# Executive Summary

Using text vectorization from scratch and pretrained vectors derived from GloVe - several Recurrent Neural Networks were adapted for Natural Language Processing use and tuned in both architecture and hyperparameters. 

Several potential use cases for the model could be imagined:
- Aiding individuals managing CKD in exploring recipes to expand their culinary limitations
- Narrowing down collections of recipes to those that are likely kidney-friendly for registered dietitians and other professionals to allow more efficient review and approval when curating recipes

Given the possibility of the former, models with high precision were favored as a defensive measure against false negatives, in which recipes that were likely not kidney friendly are mistakenly labeled as appropriate. However, accuracy was also factored in.

As such, a model with 97% precision and 80% accuracy on validation (unseen data) was selected due to its reasonable tradeoff (few models could surpass that precision, and usually at the cost of significant accuracy). Care was taken to ensure that in the validation data, no recipe variants created as part of the augmentation effort were included that had close variants in training data, thus mitigating data leakage.

# Areas for further research 

### Data:
This efficacy of this model would likely be dramatically improved by incorporating additional data, especially those recipes that are kidney friendly. In addition, text such as preparation instructions and descriptions could be analyzed as well.

### Expertise:
Nutritional thresholds, recipe label selection, and information on Bayesian Error could benefit greatly from subject matter expertise.

### Standardization 
The process of standardizing ingredient measurements could be improved, perhaps even through sub-models.

### Technique Variety
Exploration of other model types and NLP techniques could potentially improve results


# Acknowledgements and Citations

### Special Thanks:
- Presentation template was created by Slidesgo, and includes icons by Flaticon, and infographics & images by Freepik
- Tim Book -for all data science instruction, but in the context of this project: data augmentation and RNNs with NLP
- American Kidney Fund and Kidney Foundation of Canada for their curation of kidney-friendly recipes
- Jeffrey Pennington,   Richard Socher,   Christopher D. Manning for use of Global Vectors for Word Representation (GloVe)

### External Tools & Information:
- Jeffrey Pennington, Richard Socher, and Christopher D. Manning. 2014. GloVe: Global Vectors for Word Representation. [pdf] [bib]
- Brownlee, Jason. (2020, August 25). Use Early Stopping to Halt the Training of Neural Networks At the Right Time. MachineLearningMastery.com. https://machinelearningmastery.com/how-to-stop-training-deep-neural-networks-at-the-right-time-using-early-stopping/\

### Dietary Information
- National Institute of Diabetes and Digestive and Kidney Diseases (NIDDK). Sodium Tips for People with CKD. Accessed December 5, 2023. https://www.niddk.nih.gov/-/media/Files/Health-Information/Health-Professionals/Kidney-Disease/SodiumTipsforPeopleCKD EN.pdf
- National Institute of Diabetes and Digestive and Kidney Diseases (NIDDK). Eating Right for Chronic Kidney Disease. Accessed November 29, 2023. https://www.niddk.nih.gov/health-information/kidney-disease/chronic-kidney-disease-ckd/eating-nutrition
- Ko, Gang Jee, et al. "Dietary Protein Intake and Chronic Kidney Disease." Current Opinion in Clinical Nutrition and Metabolic Care 20.1 (2017): 77-85. https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6855949/
- Mayo Clinic. How to track saturated fat. [Online]. Rochester, MN: Mayo Clinic; 2023 [Updated March 03, 2023]. Available from: https://www.mayoclinic.org/healthy-lifestyle/nutrition-and-healthy-eating/in-depth/saturated-fat/art-20045858

