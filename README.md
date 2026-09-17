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

Cinq méthodologies de VaR sont étudiées : **historique, gaussienne, Student-t, Monte Carlo  et 
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

### Notebook 04 — [Backtesting de la VaR et Stress Testing](https://github.com/yeo-donignon-sekou/market-risk-analytics/blob/main/notebooks/04_backtesting_and_stress_testing.ipynb)

Ce notebook évalue la fiabilité des prévisions glissantes de VaR produites dans le Notebook 03 et analyse la résistance du portefeuille à des conditions de marché extrêmes.

Trois modèles, **VaR historique, VaR gaussienne paramétrique et VaR EWMA** sont backtestés aux seuils de confiance de **95 % et 99 %**. L'évaluation repose sur le nombre et la fréquence des exceptions, complétés par le **test de couverture inconditionnelle de Kupiec**, le **test d'indépendance de Christoffersen** et un **test de couverture conditionnelle**.

L'analyse est complétée par le **Basel Traffic Light** à 99 %, puis par des stress tests historiques et hypothétiques. Les scénarios de stress couvrent notamment les chocs actions, taux, crédit, change et matières premières, avec une attribution des pertes par position.

#### Résultats principaux

- Le backtesting de la **VaR historique à 99 %** repose sur **1 982 observations** et identifie **34 exceptions**, soit un taux d'exception observé de **1,72 %**, contre **1 %** attendu pour une VaR à 99 %.
- Pour cette VaR historique à 99 %, le **test de Kupiec est rejeté**, indiquant que la fréquence des exceptions n'est pas compatible avec le niveau de couverture attendu au seuil statistique retenu.
- Le **test d'indépendance de Christoffersen est également rejeté**, ce qui met en évidence une dépendance temporelle des exceptions. Le **test de couverture conditionnelle** conduit lui aussi au rejet du modèle.
- Malgré ces résultats statistiques, la classification **Basel Traffic Light** ressort en **zone verte** pour l'évaluation réalisée dans le notebook. Cette différence illustre que la classification réglementaire et les tests statistiques n'évaluent pas exactement les mêmes propriétés du modèle.
- Les stress tests historiques identifient le scénario **2022 Inflation and Rate Shock** comme le scénario historique le plus défavorable parmi les périodes étudiées.
- Parmi les **6 scénarios hypothétiques**, le scénario **Severe Global Crisis** génère la perte la plus importante, estimée à **400 886,25 €** sur la valeur courante du portefeuille.
- Rapportée à une valeur de portefeuille de **1 999 432,66 €**, cette perte représente environ **20,05 %** du portefeuille.
- L'analyse des pertes historiques est également étendue à plusieurs horizons de **1, 5, 10 et 20 jours de bourse**, afin d'étudier l'impact de l'allongement de l'horizon de risque.

Ces résultats montrent l'intérêt de compléter la VaR par deux niveaux d'analyse distincts : le **backtesting**, qui évalue la cohérence des prévisions avec les pertes effectivement observées, et le **stress testing**, qui mesure l'exposition du portefeuille à des scénarios extrêmes explicitement définis.

Les résultats de backtesting, les stress tests et les contributions aux pertes sont ensuite transmis au Notebook 05 pour l'analyse des facteurs de risque, des concentrations et la consolidation du reporting final.

### Notebook 05 — [Analyse des facteurs de risque et reporting final](https://github.com/yeo-donignon-sekou/market-risk-analytics/blob/main/notebooks/Risk_Factors_and_Final_Reporting.ipynb)

Ce dernier notebook complète l'analyse du risque de marché par une étude des facteurs de risque, 
des contributions au risque et de la concentration du portefeuille. Il consolide ensuite les 
résultats produits dans les notebooks précédents afin de construire un reporting synthétique du 
profil de risque du portefeuille.

L'analyse factorielle repose notamment sur une **Analyse en Composantes Principales (PCA)** appliquée 
aux rendements standardisés des actifs. Elle est complétée par le calcul de la **Marginal VaR**, de la 
**Component VaR**, des contributions à la volatilité et d'indicateurs de concentration.

#### Résultats principaux

- Les **3 premières composantes principales** expliquent **80,51 %** de la variance totale des 
  rendements standardisés : **37,25 %** pour PC1, **26,60 %** pour PC2 et **16,66 %** pour PC3.
- Les **actions américaines (US Equity)** constituent la première source de risque du portefeuille : 
  elles représentent **25 % de l'exposition**, mais contribuent à **42,22 % de la volatilité totale**.
- Elles constituent également le principal contributeur à la **Component VaR**, avec une contribution 
  de **12 153,19 €**.
- La somme des contributions individuelles conduit à une **Component VaR totale de 28 785,30 €**, 
  permettant d'attribuer le risque global du portefeuille aux différentes positions.
- L'indice de concentration **Herfindahl-Hirschman (HHI)** s'établit à **0,185**, correspondant à un 
  nombre effectif d'environ **5,41 positions**. Le niveau de concentration du portefeuille est classé 
  comme **modéré** dans le cadre retenu dans le notebook.
- Le reporting final consolide une **VaR historique à 99 % de 35 717,61 €**, une 
  **Expected Shortfall historique à 99 % de 52 141,53 €**, ainsi qu'un drawdown maximal de 
  **-22,17 %**.
- La validation du modèle de VaR historique à 99 % conserve **34 exceptions sur 1 982 observations** 
  et conduit à une évaluation **"Requires review"**, tandis que le Basel Traffic Light est en 
  **zone verte**.
- Le scénario hypothétique le plus sévère reste **Severe Global Crisis**, avec une perte estimée à 
  **400 886,25 €**, soit **20,05 %** de la valeur du portefeuille.

#### Reporting et consolidation

Le notebook rassemble les résultats du pipeline dans plusieurs formats destinés à faciliter leur 
réutilisation : **CSV, Parquet, JSON et Excel**. Il produit notamment les contributions à la 
volatilité, la Component VaR, les résultats de PCA, les mesures de concentration, les résultats de 
backtesting et les stress tests.

Le pipeline complet est finalement soumis à des contrôles de cohérence et d'intégrité afin de vérifier 
que les sorties des cinq notebooks peuvent être consolidées et réutilisées de manière cohérente.
