# Analyse par régression de Poisson du nombre de récompenses

## Présentation du projet

Ce dépôt contient le code R et les sorties graphiques utilisés dans un projet statistique portant sur la **régression de Poisson** dans le cadre des **modèles linéaires généralisés (GLM)**.

L'objectif du projet est de modéliser le nombre de récompenses obtenues par des individus à partir de leur score en mathématiques.

La variable réponse étudiée est une variable de comptage, ce qui justifie l'utilisation d'un modèle de régression de Poisson.

---

## Objectif du projet

L'objectif principal est d'expliquer le nombre de récompenses obtenues par un individu en fonction de son score en mathématiques.

La variable réponse est :

- `Awards` : nombre de récompenses obtenues par un individu.

La variable explicative est :

- `Math_Score` : score obtenu en mathématiques.

La problématique centrale est :

> Comment le score en mathématiques permet-il d'expliquer le nombre de récompenses obtenues par un individu à l'aide d'un modèle de régression de Poisson ?

---

## Jeu de données

La base de données contient 200 observations et deux variables principales :

| Variable | Description | Type |
|---|---|---|
| `Awards` | Nombre de récompenses obtenues | Variable de comptage |
| `Math_Score` | Score en mathématiques | Variable quantitative |

La variable `Awards` prend des valeurs entières positives ou nulles. Elle correspond donc à une variable de comptage, ce qui rend le modèle de Poisson approprié.

---

## Modèles statistiques estimés

Deux modèles de régression de Poisson sont estimés et comparés.

### Modèle 1 : Régression de Poisson simple

```text
log(lambda_i) = beta_0 + beta_1 Math_Score_i
```

Ce modèle suppose une relation log-linéaire entre le score en mathématiques et le nombre moyen attendu de récompenses.

### Modèle 2 : Régression de Poisson quadratique

```text
log(lambda_i) = beta_0 + beta_1 Math_Score_i + beta_2 Math_Score_i^2
```

Ce modèle permet de prendre en compte une relation non linéaire entre le score en mathématiques et le nombre moyen attendu de récompenses.

---

## Méthodologie

L'analyse suit les étapes suivantes :

1. Importation et préparation de la base de données
2. Analyse descriptive des variables
3. Vérification que `Awards` est une variable de comptage
4. Analyse graphique exploratoire
5. Division des données en échantillon d'apprentissage et échantillon de validation
6. Estimation du modèle de Poisson simple
7. Estimation du modèle de Poisson quadratique
8. Tests de significativité des coefficients
9. Tests d'adéquation par la déviance et la statistique de Pearson
10. Validation prédictive sur l'échantillon test
11. Construction de la courbe ROC
12. Comparaison des modèles et choix du modèle final

---

## Découpage apprentissage-validation

La base est divisée en deux parties :

```text
80 % pour l'échantillon d'apprentissage
20 % pour l'échantillon de validation
```

Les modèles sont estimés sur l'échantillon d'apprentissage, puis évalués sur l'échantillon de validation.

---

## Résultats principaux

Le modèle de Poisson simple montre que `Math_Score` a un effet positif et statistiquement significatif sur le nombre moyen attendu de récompenses.

Dans le modèle simple, un point supplémentaire en mathématiques augmente le nombre moyen attendu de récompenses d'environ :

```text
8.07 %
```

Le modèle quadratique améliore l'ajustement et la qualité prédictive.

Le modèle final retenu est :

```text
log(lambda_i) = -14.20 + 0.3332 Math_Score_i - 0.001810 Math_Score_i^2
```

Le terme quadratique est statistiquement significatif, ce qui indique que l'effet du score en mathématiques n'est pas strictement linéaire.

---

## Comparaison des modèles

Le modèle quadratique est retenu comme modèle final, car il améliore plusieurs critères par rapport au modèle simple.

| Critère | Modèle de Poisson simple | Modèle de Poisson quadratique |
|---|---:|---:|
| AIC | 176.4542 | 166.2959 |
| MAE | 0.2194727 | 0.127824 |
| RMSE | 0.2989070 | 0.2239236 |
| Déviance test | 9.561734 | 4.604606 |
| AUC | 0.99375 | 0.99375 |

Même si les deux modèles présentent la même AUC, le modèle quadratique est meilleur pour prédire le nombre moyen attendu de récompenses.

---

## Performance ROC

Pour construire la courbe ROC, la variable de comptage `Awards` est transformée en variable binaire :

```text
Awards_binary = 1 si Awards > 0
Awards_binary = 0 si Awards = 0
```

La probabilité prédite d'obtenir au moins une récompense est calculée par :

```text
P(Awards > 0) = 1 - exp(-lambda)
```

L'analyse ROC donne :

```text
AUC = 0.99375
```

Le seuil optimal selon l'indice de Youden est :

```text
Seuil = 0.2126067
```

Les performances de classification sont :

| Indicateur | Valeur |
|---|---:|
| Accuracy | 0.9756098 |
| Sensibilité | 1.0000000 |
| Spécificité | 0.9600000 |
| Précision | 0.9411765 |
| F1-score | 0.9696970 |

---

## Organisation du dépôt

```text
poisson-regression-awards/
│
├── data/
│   └── competition_awards_data.xlsx
├── Rapport/
│   └── rapport_regression_poisson.pdf
│
├── scripts/
│   └── poisson_regression_analysis.R
│
├── outputs/
│   ├── distribution_awards.png
│   ├── distribution_math_score.png
│   ├── relation_math_awards.png
│   ├── residus_pearson_m1.png
│   ├── observes_ajustes_m1.png
│   ├── validation_m1.png
│   ├── roc_m1.png
│   ├── roc_comparaison_m1_m2.png
│   ├── cook_m2.png
│   └── leverage_m2.png
│
└── README.md
```

---

## Reproduire les résultats

Pour reproduire l'analyse complète, ouvrir R ou RStudio puis exécuter :

```r
source("scripts/poisson_regression_analysis.R")
```

La base de données doit être placée dans le dossier `data/`.

Les packages R nécessaires sont :

```r
readxl
dplyr
ggplot2
pROC
```

Si un package n'est pas installé, le script peut l'installer automatiquement.

---

## Conclusion finale

Le modèle de régression de Poisson est adapté à cette étude, car la variable réponse `Awards` est une variable de comptage.

Le modèle de Poisson quadratique est retenu comme modèle final, car il présente un meilleur ajustement et de meilleures performances prédictives que le modèle de Poisson simple.

Les résultats montrent que le score en mathématiques a un effet positif important sur le nombre moyen attendu de récompenses, mais que cet effet diminue progressivement lorsque le score devient élevé.

---

## Auteur

Projet réalisé dans le cadre d'un TP sur les modèles linéaires généralisés et la régression de Poisson.
