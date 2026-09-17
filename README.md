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

### Notebook 02 — [Construction du portefeuille et analyse du P&L](https://github.com/yeo-donignon-sekou/market-risk-analytics/blob/main/notebooks/02_portfolio_and_pnl_analysis.ipynb)

Ce notebook construit un portefeuille multi-actifs à partir des données de marché préparées dans le Notebook 01. 
Le portefeuille, d'une valeur initiale de **1 000 000 €**, est composé de six positions représentant plusieurs 
facteurs de risque : actions américaines et européennes, taux d'intérêt, crédit, matières premières et change.

Les allocations initiales sont converties en quantités d'actifs puis valorisées quotidiennement selon une stratégie 
buy-and-hold. Le notebook calcule la valeur du portefeuille, les rendements, le P&L quotidien et cumulé, les poids 
effectifs des positions ainsi que leur contribution individuelle au P&L. L'analyse est complétée par le suivi du 
drawdown et de la volatilité glissante.

#### Résultats principaux

- Sur **2 232 observations de rendements**, la valeur du portefeuille passe de **1 000 000 €** à 
  **1 999 432,66 €**, soit un P&L cumulé de **999 432,66 €** sur la période étudiée.
- La volatilité annualisée du portefeuille s'établit à **10,39 %**, tandis que le drawdown maximal atteint 
  **-22,17 %**.
- Le pire P&L quotidien est enregistré le **12 mars 2020**, avec une perte de **58 860,17 €**. Les actions 
  américaines contribuent à **43,07 %** de cette perte et les actions européennes à **34,57 %**.
- Sur l'ensemble de la période, les actions américaines représentent la principale contribution au P&L avec 
  **534 961,36 €**, soit **53,53 %** du P&L total. Elles sont suivies par les actions européennes avec 
  **226 407,59 € (22,65 %)** et l'or avec **195 141,82 € (19,53 %)**.
- Les Treasuries américains apportent une contribution positive de **18 017,96 €**, tandis que l'exposition 
  EUR/USD contribue négativement au résultat à hauteur de **-5 059,87 €**.
- Les contributions individuelles au P&L sont réconciliées avec le P&L global du portefeuille afin de vérifier 
  la cohérence des calculs.

Les séries de valeur du portefeuille, rendements, P&L et contributions par position sont ensuite enregistrées 
et utilisées comme données d'entrée pour le Notebook 03 consacré à la mesure du risque par VaR et Expected Shortfall.

### Notebook 03 — [Value at Risk et Expected Shortfall](https://github.com/yeo-donignon-sekou/market-risk-analytics/blob/main/notebooks/03_var_and_expected_shortfall.ipynb)

Ce notebook est consacré à la mesure quantitative du risque de marché du portefeuille construit précédemment. 
L'objectif est d'estimer les pertes potentielles à horizon **1 jour** et de comparer plusieurs approches de 
Value at Risk (VaR) et d'Expected Shortfall (ES).

Cinq méthodologies de VaR sont étudiées : **historique, gaussienne, Student-t, Monte Carlo Student-t et 
EWMA gaussienne**, aux niveaux de confiance de **95 %, 97,5 % et 99 %**. L'Expected Shortfall est également 
calculée afin de compléter la VaR par une mesure de la sévérité moyenne des pertes situées au-delà du seuil.

L'analyse statique est complétée par la construction de **prévisions glissantes de risque à horizon 1 jour**, 
calculées sur une fenêtre historique de **250 observations**. Cette approche permet de produire des mesures 
de risque utilisant uniquement l'information disponible avant chaque date d'évaluation et prépare le 
backtesting des modèles dans le notebook suivant.

#### Résultats principaux

- Les rendements du portefeuille présentent une volatilité annualisée de **10,39 %**, une asymétrie de 
  **-0,24** et une kurtosis excédentaire de **10,31**, indiquant des queues de distribution nettement plus 
  importantes que celles d'une distribution normale.
- L'ajustement d'une loi de Student conduit à **3,32 degrés de liberté**, ce qui confirme l'intérêt d'une 
  distribution à queues épaisses pour modéliser les rendements du portefeuille.
- À **99 %**, la VaR historique est la plus élevée parmi les modèles étudiés, avec **35 717,61 €**, contre 
  **34 621,20 €** pour la Student-t, **34 442,08 €** pour Monte Carlo Student-t, **29 900,82 €** pour 
  l'EWMA gaussienne et **29 774,89 €** pour l'approche gaussienne.
- L'Expected Shortfall historique à **99 %** atteint **52 141,53 €**, contre une VaR historique de 
  **35 717,61 €**. La perte moyenne au-delà du seuil de VaR est ainsi environ **46 % supérieure** au niveau 
  de VaR lui-même.
- À **99 %**, l'Expected Shortfall obtenue avec la Student-t atteint **51 703,74 €**, contre seulement 
  **34 208,70 €** avec l'hypothèse gaussienne. Cet écart met en évidence la sensibilité de la mesure du 
  risque extrême au choix de la distribution.
- Le pire rendement journalier observé dans l'échantillon est de **-5,62 %**, correspondant à une perte 
  de **58 860,17 €** le **12 mars 2020**.
- Les prévisions glissantes sont produites à partir d'une fenêtre de **250 jours**, générant 
  **1 982 observations** exploitables pour l'évaluation hors échantillon des modèles.

Les séries de VaR, d'Expected Shortfall et les prévisions glissantes produites dans ce notebook sont ensuite 
utilisées pour le backtesting et la validation statistique des modèles.
