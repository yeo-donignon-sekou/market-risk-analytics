# market-risk-analytics
Quantitative market risk analysis: VaR, Expected Shortfall, backtesting, stress testing and risk factor analysis.

# Analyse quantitative du risque de marché

## Présentation du projet

Ce projet porte sur l’analyse quantitative du risque de marché d’un portefeuille
multi-actifs d’une valeur initiale de 1 000 000 €.

L’objectif est de construire une analyse complète du risque, depuis la préparation
des données de marché et la valorisation du portefeuille jusqu’à la mesure du risque,
la validation des modèles, les stress tests et l’analyse des facteurs de risque.

Plusieurs approches sont utilisées pour estimer la Value at Risk (VaR) et
l’Expected Shortfall (ES) : méthode historique, approche paramétrique gaussienne,
loi de Student, simulation Monte Carlo et modèle EWMA.

Les modèles de VaR sont ensuite évalués à partir de prévisions glissantes à horizon
1 jour et de procédures de backtesting. L’analyse est complétée par des stress tests
historiques et hypothétiques ainsi que par une étude des principaux facteurs et
contributions au risque du portefeuille.

Le projet est organisé en cinq notebooks correspondant aux différentes étapes
de l’analyse.

### Notebook 01 — [Collecte, validation et analyse des données de marché](https://github.com/yeo-donignon-sekou/market-risk-analytics/blob/main/notebooks/01_data_collection_and_market_analysis.ipynb)

Ce premier notebook constitue la base de données de marché utilisée dans la suite du projet. 
L'analyse porte sur six instruments représentant cinq grandes sources de risque : actions américaines 
et européennes, taux d'intérêt, crédit, matières premières et change. Les données couvrent la période 
de janvier 2018 à juillet 2026.

Les séries de prix sont contrôlées, nettoyées et alignées avant le calcul des rendements simples et 
logarithmiques. L'analyse exploratoire porte ensuite sur les performances, les volatilités, les 
distributions de rendements, les corrélations entre actifs et les drawdowns.

#### Résultats principaux

- **2 232 observations de rendements** sont obtenues après nettoyage et alignement des séries de marché.
- Les **actions européennes** présentent la volatilité annualisée la plus élevée de l'échantillon, 
  à **21,40 %**, devant les actions américaines (**18,78 %**) et l'or (**16,55 %**).
- Les actifs obligataires sont moins volatils : **6,77 %** pour les Treasuries américains et 
  **9,03 %** pour les obligations corporate.
- La corrélation la plus élevée entre deux actifs distincts est observée entre les **actions américaines 
  et européennes : 0,81**. Les Treasuries américains et les obligations corporate présentent également 
  une corrélation importante de **0,70**.
- Les drawdowns maximaux atteignent **-33,72 %** pour les actions américaines et **-39,69 %** pour 
  les actions européennes, tous deux observés en mars 2020.
- Les rendements présentent des distributions non gaussiennes, avec notamment une kurtosis excédentaire 
  de **13,79** pour les actions américaines et de **27,83** pour les obligations corporate, ce qui 
  met en évidence la présence d'événements extrêmes dans les données.

Les données nettoyées et les rendements produits dans ce notebook constituent les entrées des étapes 
suivantes consacrées à la construction du portefeuille et à la mesure du risque.
