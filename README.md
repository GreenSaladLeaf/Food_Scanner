# Food Scanner

A Gradio app that identifies a packaged food from a photo of its front, predicts its processing level (NOVA group), estimates its glycaemic index (GI band and approximate value), lists its ingredients and additive types, and suggests a less processed or lower-GI alternative. This repository holds the code for the final report.

All notebooks were run on Kaggle. The data are Kaggle datasets and are not included here.

The English-speaking subset (71,298 products) is `off_filtered.csv` in the `cleaned-off` Kaggle dataset, selected with the country list in the commented cell of `cleaning-off`.

## Notebooks

Listed in the order they were run.

### Data
| Notebook | What it does |
|---|---|
| `cleaning_foodprox` | Cleans the FoodProX data (10,499 to 8,238 foods) and converts the log-scaled nutrients back to grams. |
| `cleaning-off` | Cleans the Open Food Facts export (1,350,091 to 376,917 products): required fields, range checks, fibre adjustment. |
| `foodprox_match_off` | Matches FoodProX foods to OFF products with RapidFuzz and MiniLM, plus a nutrient check (1,884 labelled products). |
| `cleaning_atkinson` | Cleans the Atkinson GI tables (4,017 rows to 3,407 healthy-subject rows to 3,061 foods). The tables were parsed from the PDF in a separate step, and that notebook is not included. |
| `atkinson_matching_fuzzy` | Shortens and merges Atkinson names, then matches them to OFF products with TF-IDF and RapidFuzz (6,851 products from 629 foods). |

### NOVA (processing level)
| Notebook | What it does |
|---|---|
| `nova_model_tf_idf` | TF-IDF + logistic regression baseline; also creates the grouped folds file used by all NOVA models. |
| `nova_match_audit` | Hand check of 50 random FoodProX–OFF pairs. |
| `nova_mlp` | Frozen MiniLM embeddings + small MLP. |
| `nova_distilbert` | Fine-tuned DistilBERT on ingredient text (out-of-fold probabilities). |
| `nova_probs_for_gi` | NOVA probabilities for the GI products, used by the fused GI model. |
| `nova_gbm_check` | Gradient-boosted models combining text probabilities with FoodProX or OFF nutrients; per-class results, confusion matrix and baby-food check. |
| `nova_both_pairs` | Retrains DistilBERT + GBM on both-method pairs only, applies the adoption rule, and makes the NOVA predictions for the app catalogue. |

### GI (glycaemic index)
| Notebook | What it does |
|---|---|
| `gi_model_new` | Baseline, plain and fused MLPs (fixed and tuned), GBM regressor and name-based kNN. |
| `gi_category_test` | Adds the OFF food category to the regressor, compares band classifier vs banded regression, and draws the predicted-vs-true plot. |
| `catalogue_gi_v2` | Trains the final GI band classifier and regressor and predicts GI for the app catalogue. |

### Product identification
| Notebook | What it does |
|---|---|
| `build_index` | Embeds all catalogue images with DINOv2-small, builds the FAISS index and runs the altered-image retrieval test. |
| `app_retrieval_clean` | Compares picture search and word search on 27 test photos and develops the recommendation rule. Outputs (photo grids) were removed to keep the file under GitHub's size limit; results are reported in Section 6.3 of the report. |

### Label reading (tested, not used in the app)
| Notebook | What it does |
|---|---|
| `ocr` | EasyOCR on ingredient lists and a rule-based nutrition panel parser (development and test photos). |
| `vlm_nutrition` | Qwen2-VL-2B-Instruct reading nutrition panels, with the frozen prompt and clear/hard photo labels. |

### App
| Notebook | What it does |
|---|---|
| `food_scanner_app` | The Gradio app: photo to five candidates, name search fallback, NOVA, GI, ingredients, additive types and alternatives. |

## Running the app
Open `food_scanner_app` on Kaggle with a GPU (T4) and Internet on. Add these inputs: the `09-build-index` notebook output, the `catalogue-predictions-v3` dataset and the `cleaned-off` dataset. Then run all cells. The last cell prints a temporary public link to the app.

The outputs are model estimates for guidance only, not medical advice.
