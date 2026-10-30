# Next Drink Type Prediction · BSH (Bosch/Siemens)

> Predicting which coffee drink a connected machine will brew next, based on its recent usage history.

**AI Project course · Deggendorf Institute of Technology (Germany) · 2026**
**Industry partner:** BSH Home Appliances (Bosch/Siemens)
**My role:** Scrum Master of the international team (sprint planning, dailies, reviews, stakeholder communication in English)

| Accuracy | Top-3 accuracy | Weighted F1 | vs. baseline | Macro F1 gain |
|---|---|---|---|---|
| **78.6%** | **87.9%** | **0.81** | **3.7×** | **21×** (0.02 → 0.43) |

## The problem
A multi-class classification problem on sequential data: given the recent brewing history of a machine, predict the next drink. The model has to learn the habits and time patterns of each machine, not just guess the most popular drinks.

## Real Big Data
- **83.3 million brews** from **216,000 machines** (~8.8 GB on disk)
- Loading everything with the engineered features would need 50–80 GB of RAM
- The pipeline runs **out-of-core with DuckDB**, streaming directly from disk
- Training used a representative sample of **2.8 million rows**

## Feature engineering (33 features)
- **Sequences:** windows of up to the last 15 drinks
- **Time context:** period of the day, weekday, month, weekend, first drink of the day, time since last brew
- **Usage profile:** favourite drink, morning/evening preference, drink variety, repetition rate

## Model
- **3-layer Bidirectional GRU** (~3M parameters): reads the drink history in both directions before predicting the next drink
- Trained on a Kaggle T4 GPU in **62.6 minutes**, early stopping at epoch 11

## Results
- **78.6% accuracy** and **87.9% top-3 accuracy**: the right drink is among the first three guesses almost 9 times out of 10
- A naive "most common drinks" baseline reaches only 21.5% → the GRU is **3.7× better**
- **Macro F1 jumped 21×** (0.02 → 0.43): the model learned to predict rare drinks too, not only the most frequent ones

> Code and data are kept private because the dataset belongs to BSH. This repository documents the approach and results.

---

## 🇧🇷 Resumo em português
Previsão da próxima bebida que uma máquina de café conectada vai preparar, a partir do histórico de uso. Com **83,3 milhões de preparos de 216 mil máquinas**, o pipeline processou os dados em disco com DuckDB e treinou uma **GRU bidirecional** de 3 camadas. Resultado: **78,6% de acurácia**, **87,9% no Top-3** e desempenho **3,7 vezes melhor que o baseline**. Fui Scrum Master do time internacional.
