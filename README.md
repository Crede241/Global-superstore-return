# 📦 Dashboard Retours de Commande — Tableau Desktop

![Tableau](https://img.shields.io/badge/Tableau-Desktop-E97627?style=flat&logo=tableau&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.x-3776AB?style=flat&logo=python&logoColor=white)
![CSV](https://img.shields.io/badge/Data-CSV-green?style=flat)
![Status](https://img.shields.io/badge/Status-Terminé-success?style=flat)

> Dashboard interactif d'analyse des retours clients par région, motif et canal de vente — construit avec Tableau Desktop sur un jeu de données de 2 000 lignes couvrant 12 mois.

---

## 📸 Aperçu

![Dashboard Retours de Commande](dashboard_retours.png)

---

## 🎯 Objectif

Analyser les retours de commande pour identifier :
- Les **motifs les plus récurrents** par région
- Le **coût brut des retours** engendré par l'entreprise
- Les **canaux de vente** et **catégories de produits** les plus touchés
- Les **tendances mensuelles** sur 12 mois

---

## 📊 Contenu du dashboard

| Vue | Type | Description |
|---|---|---|
| KPI Cards | Indicateurs | Nb retours, Coût brut, Délai moyen, Taux de retour |
| Retours par catégorie | Treemap | % de retours par catégorie produit |
| Évolution mensuelle | Courbe double axe | Nb retours + Montant € par mois |
| Statut par canal de vente | Barres empilées | Répartition des statuts par canal |
| Retours par région | Carte à bulles | Volume par région française |
| Motifs par région | Heatmap | Croisement région × motif avec coût |

---

## 🗂 Structure du projet

```
dashboard-retours/
│
├── retours_commandes.csv       # Jeu de données (2 000 lignes)
├── dictionnaire_donnees.docx   # Dictionnaire des données (16 champs)
├── dashboard_retours.png       # Capture d'écran du dashboard
└── README.md
```

---

## 📁 Dictionnaire des données

Le fichier `retours_commandes.csv` contient **16 colonnes** séparées par des **points-virgules** :

| Champ | Type | Description |
|---|---|---|
| `order_id` | Texte | Identifiant unique de la commande |
| `return_id` | Texte | Identifiant unique du retour |
| `order_date` | Date | Date de la commande (YYYY-MM-DD) |
| `return_date` | Date | Date du retour (YYYY-MM-DD) |
| `return_delay_days` | Entier | Délai en jours entre commande et retour |
| `product_name` | Texte | Nom du produit retourné |
| `category` | Texte | Catégorie produit (5 valeurs) |
| `quantity` | Entier | Nombre d'unités retournées |
| `unit_price` | Décimal | Prix unitaire en € |
| `return_amount` | Décimal | Montant total du retour (unit_price × quantity) |
| `return_reason` | Texte | Motif du retour (7 valeurs) |
| `return_status` | Texte | Statut du retour (4 valeurs) |
| `sales_channel` | Texte | Canal de vente (4 valeurs) |
| `carrier` | Texte | Transporteur (5 valeurs) |
| `region` | Texte | Région française du client |
| `city` | Texte | Ville du client |

---

## 🔢 KPIs principaux

| Indicateur | Valeur | Formule Tableau |
|---|---|---|
| Retours totaux | 2 000 | `COUNTD([return_id])` |
| Coût brut des retours | 710 618 € | `SUM([return_amount])` |
| Délai moyen de retour | 16 jours | `AVG([return_delay_days])` |
| Taux de retour | 18,2% | `COUNTD([return_id]) / TOTAL(COUNTD([order_id]))` |

---

## 💡 Insights clés

- **PACA** concentre le coût le plus élevé avec **22 257 €** sur le motif *Mauvaise taille / couleur* — révèle un problème de description produit ou de guide des tailles sur ce marché
- **Produit défectueux** est le motif le plus récurrent en **Auvergne-Rhône-Alpes** et **Pays de la Loire** — potentiellement lié à un problème qualité transporteur
- La **Normandie** présente un pic inhabituel sur les commandes en double (**9 669 € pour 16 cas**) — montants unitaires très élevés à investiguer
- Le motif **Mauvaise taille / couleur** est le plus coûteux globalement, présent dans toutes les régions

---

## ⚙️ Champs calculés Tableau

```
// Taux de retour
COUNTD([return_id]) / TOTAL(COUNTD([order_id]))

// % par catégorie (Treemap)
COUNTD([return_id]) / TOTAL(COUNTD([return_id]))

// Titre dynamique
ATTR([region])

// Latitude région (carte)
IF [region] = "Île-de-France" THEN 48.8566
ELSEIF [region] = "Auvergne-Rhône-Alpes" THEN 45.7597
ELSEIF [region] = "Nouvelle-Aquitaine" THEN 44.8378
// ...
END
```

---

## 🚀 Utilisation

### Prérequis
- Tableau Desktop (toute version récente)

### Connexion des données
1. Ouvrir Tableau Desktop
2. **Se connecter** → **Fichier texte** → sélectionner `retours_commandes.csv`
3. Séparateur : **point-virgule (;)**
4. Vérifier que `order_date` et `return_date` sont bien en type **Date**

### Recréer le dashboard
Suivre la structure décrite dans la section **Contenu du dashboard** ci-dessus. Chaque vue est une feuille séparée assemblée dans un dashboard unique avec filtres globaux et actions de filtre croisées.

---

## 🛠 Génération des données

Le fichier CSV a été généré avec Python :

```python
import csv, random
from datetime import datetime, timedelta

# 2 000 retours sur 12 mois
# 5 catégories · 7 motifs · 4 statuts · 4 canaux · 10 régions françaises
```

---

## 📬 Contact

Projet réalisé dans le cadre d'un portfolio Data Analyst.  
N'hésitez pas à ouvrir une **issue** ou à me contacter pour toute question.
