<h1 align="center">🎯 SmartRecruit</h1>
<p align="center"><b>Matching intelligent entre CV et offres d'emploi</b></p>
<p align="center">
  <img src="https://img.shields.io/badge/Python-3.x-blue" />
  <img src="https://img.shields.io/badge/NLP-spaCy-green" />
  <img src="https://img.shields.io/badge/ML-scikit--learn-orange" />
  <img src="https://img.shields.io/badge/BI-Power%20BI-yellow" />
  <img src="https://img.shields.io/badge/DB-SQLite-lightgrey" />
</p>

---

## 📌 Présentation

**SmartRecruit** est une solution d'intelligence artificielle qui analyse automatiquement des CV et des offres d'emploi afin de calculer un **score de compatibilité** entre chaque candidat et chaque poste, de classer les candidats, et de restituer les résultats dans un **tableau de bord interactif**.

> Pour une seule offre d'emploi, un recruteur peut recevoir des centaines de CV. Le tri manuel est long et subjectif. SmartRecruit automatise cette tâche de manière **objective, reproductible et explicable**.

| | |
|---|---|
| 📄 CV analysés | **224** |
| 💼 Offres d'emploi | **18** |
| 🔗 Comparaisons candidat–offre | **4 032** |
| 🎯 Précision du modèle | **97 %** |

---

## 🧠 Pipeline technique

```
CV (.docx) + Offres d'emploi
        │
        ▼
1. Nettoyage du texte  ──►  minuscules, stopwords, lemmatisation (spaCy)
        │
        ▼
2. Extraction structurée  ──►  compétences, expérience, formation
        │
        ▼
3. Vectorisation TF-IDF
        │
        ▼
4. Similarité cosinus  ──►  matrice 224 × 18
        │
        ▼
5. Score combiné pondéré
        │
        ▼
6. Classification (Régression logistique)
        │
        ▼
7. Base SQLite  ──►  8. Tableau de bord Power BI
```

**Formule du score combiné :**

```
score_final = 0,40 × similarité_texte
            + 0,35 × recouvrement_compétences
            + 0,15 × adéquation_expérience
            + 0,10 × adéquation_formation
```

---

## 🛠️ Technologies utilisées

| Technologie | Rôle |
|-------------|------|
| **Python 3** | Langage principal du pipeline |
| **Pandas** | Manipulation des données |
| **python-docx** | Lecture des CV (.docx) |
| **spaCy** | Nettoyage NLP, lemmatisation, NER |
| **scikit-learn** | TF-IDF, similarité cosinus, régression logistique |
| **BeautifulSoup** | Nettoyage HTML des offres |
| **SQLite** | Base de données relationnelle |
| **Power BI** | Tableau de bord interactif |

---

## 📂 Structure du projet

```
SmartRecruit/
├── notebooks/
│   ├── 01_extraction.ipynb      # Lecture CV, nettoyage NLP, extraction
│   ├── 02_job_offers.ipynb      # Traitement des offres d'emploi
│   ├── 03_matching.ipynb        # TF-IDF, similarité, score, classification
│   └── 04_database.ipynb        # Base SQLite + export Power BI
├── data/
│   └── processed/               # Données traitées (CSV)
├── sql/
│   └── smartrecruit.db          # Base de données SQLite
├── dashboard/
│   ├── *.csv                    # Exports pour Power BI
│   └── SmartRecruit.pbix        # Tableau de bord Power BI
├── Rapport_PFA_SmartRecruit.pdf # Rapport complet
└── README.md
```

---

## ⚙️ Étapes du projet

### 1️⃣ Extraction des CV
Lecture des 224 fichiers Word avec `python-docx`, puis nettoyage du texte avec **spaCy** (minuscules, suppression des mots vides, lemmatisation).

### 2️⃣ Extraction d'informations structurées
- **Compétences** — détectées via une liste de **171 mots-clés** (Java, Agile, SQL, PMP…) — 47 compétences/CV en moyenne
- **Années d'expérience** — extraites par expressions régulières — 207/224 CV
- **Formation** — niveau de diplôme hiérarchisé — 189/224 CV

### 3️⃣ Traitement des offres d'emploi
Filtrage d'un dataset Kaggle (30 002 annonces) vers **18 offres** pertinentes et équilibrées, nettoyage HTML des descriptions avec **BeautifulSoup**.

### 4️⃣ Moteur de matching
Vectorisation **TF-IDF** de tous les documents, calcul de la **similarité cosinus** entre chaque CV et chaque offre, puis agrégation en un **score combiné pondéré**.

### 5️⃣ Classification
Modèle de **régression logistique** entraîné pour prédire la compatibilité (compatible / non compatible) — **97 % de précision**.

### 6️⃣ Base de données
Stockage dans **SQLite** avec 3 tables liées : `cvs`, `offres`, `matches`.

### 7️⃣ Tableau de bord Power BI

**Page 1 — Vue d'ensemble RH**
![Vue d'ensemble](Capture%20d%27%C3%A9cran%202026-07-22%20001016.png)
> Indicateurs clés, score moyen par poste, répartition compatibles / non compatibles, classement des candidats.

**Page 2 — Fiche candidat**
![Fiche candidat](Capture%20d%27%C3%A9cran%202026-07-22%20001046.png)
> Profil du candidat, compétences, carte géographique, scores face aux 18 offres.

**Page 3 — Vue par offre**
![Vue par offre](Capture%20d%27%C3%A9cran%202026-07-22%20001127.png)
> Sélection poste + candidat, jauge de compatibilité, classement des meilleurs candidats.

---

## 📊 Résultats

| Métrique | Valeur |
|----------|--------|
| Accuracy | **97 %** |
| Précision (compatible) | **100 %** |
| Rappel (compatible) | **95 %** |
| F1-score | **97 %** |

---

## 🚀 Installation

```bash
pip install pandas python-docx spacy scikit-learn beautifulsoup4
python -m spacy download en_core_web_sm
```

Puis exécuter les notebooks dans l'ordre : `01` → `02` → `03` → `04`.

---

## 📝 Note méthodologique

Les champs de **localisation** et **d'université** affichés dans le tableau de bord sont des données de démonstration destinées à enrichir la visualisation. Ils **n'entrent pas** dans le calcul du score de compatibilité, qui repose exclusivement sur les données réellement extraites des CV (texte, compétences, expérience, formation).

---

## 📄 Rapport

Le rapport complet du projet (PFA) est disponible ici : [`Rapport_PFA_SmartRecruit.pdf`](Rapport_PFA_SmartRecruit.pdf)

---

<p align="center">Projet de Fin d'Année — Intelligence Artificielle & Data Analysis</p>
