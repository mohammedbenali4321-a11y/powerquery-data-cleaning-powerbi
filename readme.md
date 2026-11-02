# 📊 TP1 Power Query (Power BI) - Nettoyage et Préparation des Données

Ce projet s'inscrit dans le cadre de la préparation et de l'intégration de données de ventes hétérogènes pour l'entreprise **TechMaroc SARL**, distributeur de matériel informatique présent dans 6 villes du Maroc. 

L'objectif principal est de consolider, nettoyer et fiabiliser des données brutes provenant de sources multiples (Excel et CSV) afin de permettre la création d'un tableau de bord de pilotage des ventes pour le 1er semestre 2025.

---

## 🎯 Objectifs du Projet

- **Connexion multi-sources :** Extraction et transformation de données issues d'un fichier Excel multi-feuilles (`Ventes_T1`, `Ventes_T2`, `Objectifs`) et d'un catalogue produits au format CSV (`Produits.csv`).
- **Diagnostic de la qualité des données :** Détection des anomalies via les outils de profilage de Power Query (Qualité, Distribution et Profil de colonne).
- **Transformations & Nettoyage M :** Application de plus de 10 types de transformations complexes (gestion des types, suppression des doublons, remplissage vers le bas, parsing de dates, délimitation, manipulation de chaînes et correction d'erreurs).
- **Modélisation & Automatisation :** 
  - Combinaison des requêtes par **Ajout** (`Ventes_T1` + `Ventes_T2` $\rightarrow$ `Ventes_S1`) et par **Fusion** (Jointure externe gauche avec la table `Produits`).
  - Dépivotage (*Unpivot*) de la table `Objectifs` pour obtenir un format normalisé.
  - Création de colonnes calculées en **langage M** (`Taux Remise`, `Montant HT`).

---

## 🛠️ Stack Technique

* **Outil ETL & Visualization :** Power BI Desktop / Power Query Editor
* **Langages :** M (Power Query Formula Language), DAX (pour les mesures fondamentales)
* **Formats de données :** Microsoft Excel (`.xlsx`), CSV (`.csv`)

---

## 📑 Structure du Projet

```text
.
├── Data/
│   ├── Ventes_TechMaroc_2025.xlsx  # Extractions brutes ERP (Ventes_T1, Ventes_T2, Objectifs)
│   └── Produits.csv               # Catalogue produits (UTF-8, séparateur ;)
├── TP1_PowerQuery_Stagiaires.pdf   # Fiche des consignes & grille d'auto-évaluation
├── tomas Idisson.pbix, tomasa.pbix       # Fichier projet Ventes_1 & Ventes_2
├── tomas Idisson Version 1.pbix        # Fichier projet Power BI finalisé
└── README.md                       # Documentation du projet
```

---

## 🔄 Étapes de Nettoyage & Transformation

1. **Structure & En-têtes :** Suppression des lignes de titre ERP et promotion des vrais en-têtes. Filtrage dynamique pour exclure les lignes de totalisation (`TOTAL`).
2. **Propagation des données (`Vendeur`) :** Application du remplissage vers le bas (*Fill Down*) sur les blocs de transactions.
3. **Harmonisation des Entités :**
   - **Villes :** Remplacement des variantes orthographiques (`Casa` $\rightarrow$ `Casablanca`, `Fes` $\rightarrow$ `Fès`, `Marrakesh` $\rightarrow$ `Marrakech`, `Tangier` $\rightarrow$ `Tanger`) pour obtenir exactement 6 villes distinctes.
   - **Clients :** Fractionnement de la colonne par délimiteur (`-`) en `Code Client` et `Nom Client`, suppression des espaces parasites et conversion des noms en *Capitalize Each Word*.
4. **Nettoyage Métrique :**
   - **Quantité :** Conversion en nombre entier et exclusion des valeurs `null` non valorisables.
   - **Prix Unitaire :** Extraction du texte (`MAD`), nettoyage des espaces insécables et conversion en `Nombre décimal` (paramètres régionaux Français - Maroc).
5. **Formules Personnalisées (Langage M) :**
   - **Taux Remise :** Gestion hybride des formats (valeurs numériques, pourcentages au format texte comme `"5%"` ou cellules vides) :
     ```m
     if [Remise] = null then 0
     else if Value.Is([Remise], type text)
     then Number.From(Text.Remove([Remise], "%")) / 100
     else [Remise]
     ```
   - **Montant HT :** Calcul valorisé de la commande :
     ```m
     [Quantité] * [Prix Unitaire] * (1 - [Taux Remise])
     ```
6. **Dépivotage (`Objectifs`) :** Transformation des colonnes mensuelles en lignes pour obtenir une structure relationnelle propre (`Ville`, `Mois`, `Objectif (MAD)`).

---

## 👤 Auteur & Liens Professionnels

**Mohamed BEN ALI**  
*Data Analyst / Business Intelligence Specialist*

- 💼 **LinkedIn :** [Mohamed BEN ALI](https://www.linkedin.com/in/mohamed-ben-ali-m)
- 🐙 **GitHub :** [@mohammedbenali4321-a11y](https://github.com/mohammedbenali4321-a11y)
- 📊 **Kaggle :** [Mohamed BEN ALI](https://www.kaggle.com/mohamedbenali)
- 🗺️ **Roadmap.sh :** [Mohamed BEN ALI - Skill Profile](roadmap.sh/u/benalimohamed)
