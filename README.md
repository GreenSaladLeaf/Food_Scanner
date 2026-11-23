# Food scanner – code for the final report

All notebooks were run on Kaggle. The data (Open Food Facts, FoodProX, Atkinson GI tables, test photos) are Kaggle datasets and are not included here.

- **Data:** cleaning-off, FoodProX matching, 12 (match audit), Atkinson matching
- **NOVA:** 04 (TF-IDF), nova-mlp (MiniLM), 05 (DistilBERT), 06 (probabilities for GI), 07 (GBM), 23 (both-method pairs, app model)
- **GI:** gi-model-new (MLPs, GBM, kNN), 18 (food category and bands), 19 (final models and catalogue predictions)
- **Identification:** 09 (picture index), 20 (27-photo test)
- **Label reading:** 10 (OCR), 11 (VLM)
- **App:** 22 (Gradio app)

The English-speaking subset (71,298 products) uses the country list in the commented cell of cleaning-off. It is applied in the later notebooks.
