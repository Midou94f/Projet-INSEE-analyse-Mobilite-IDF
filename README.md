# Projet INSEE – Analyse de la Mobilité en Île-de-France  
**Modélisation économétrique des choix de mobilité – Nested Logit – Construction d’un jeu de données consolidé**

---

##  1. Objectif général du projet

Ce projet vise à analyser les comportements de mobilité en Île-de-France en combinant :

- un **travail approfondi de préparation et fusion de données**,  
- une **analyse statistique descriptive** de la population étudiée,  
- une **modélisation économétrique rigoureuse** des choix individuels à l’aide d’un **modèle de type Nested Logit**.

L’enjeu central : **comprendre quels facteurs socio-démographiques influencent les choix de mobilité**, et comment les caractéristiques individuelles et territoriales structurent ces décisions.

---

##  2. Structure du dépôt

## 3. Construction et préparation du jeu de données

Le travail repose sur un **pipeline de nettoyage robuste**, comprenant :

### ✔ Harmonisation des variables
- Standardisation des identifiants
- Recodage des modalités (zones, statuts, situation résidentielle)
- Gestion des valeurs manquantes

### ✔ Fusion de plusieurs sources INSEE
- Fichiers individus  
- Fichiers mobilité  
- Dictionnaires de métadonnées

### ✔ Production de jeux consolidés
Export final au format **Parquet** pour des performances optimales :

- `mono_clean.parquet`  
- `single_clean.parquet`

Ces fichiers servent de base à toutes les analyses statistiques et économétriques.

---

##  4. Analyse descriptive

L’analyse statistique couvre notamment :

### • Caractéristiques socio-démographiques
- Âge  
- Sexe  
- Niveau d’éducation  
- Statut professionnel  
- Catégorie socioprofessionnelle  

### • Indicateurs territoriaux
- Département de résidence  
- Département de travail  
- Flux domicile-travail  
- Distance moyenne / structure des déplacements

### • Distributions clés
- Répartition des travailleurs mobiles vs non mobiles  
- Analyse des zones attractives  
- Profil comparatif des travailleurs selon leurs regimes de mobilité

Ces analyses permettent de **valider la cohérence des données** et de **justifier la structure du modèle de choix discrets**.

---

## 5. Modélisation économétrique : Nested Logit

Le cœur du projet repose sur un **modèle économétrique de choix discrets**, adapté à des décisions comportant une structure hiérarchique :

###  Pourquoi un Nested Logit ?
Le choix individuel peut se représenter en **niveaux imbriqués** :

1. **Niveau 1 : type de mobilité**  
   - Mobilité locale  
   - Mobilité intra-départementale  
   - Mobilité inter-départements  

2. **Niveau 2 : choix spécifique de zone / déplacement**  
   → Déterminé par les caractéristiques individuelles et territoriales.

Le Nested Logit permet de modéliser **la corrélation intra-nœud** entre alternatives proches (non satisfaction de l’hypothèse IIA du Logit simple).

---

## 6. Spécification du modèle

Le modèle estimé dans `projet_nestedlogit_mehdi.ipynb` inclut des variables explicatives telles que :

### ✔ Variables individuelles :
- Âge  
- Niveau d’éducation  
- Compétences numériques  
- Statut marital  
- Catégorie socioprofessionnelle  

### ✔ Variables économiques :
- Niveau de revenu estimé  
- Situation professionnelle  
- Secteur d’activité  

### ✔ Variables territoriales :
- Distance domicile-travail  
- Département de résidence  
- Attractivité des zones (emplois, infrastructures)

---

##  7. Interprétation économétrique

Le modèle fournit :

### ✔ Effets marginaux :
Impact d’une variation unitaire d’une caractéristique individuelle sur la probabilité de choisir un type de mobilité.

### ✔ Paramètres des nœuds (inclusive values) :
Permettent d’évaluer :
- la cohérence des regroupements d’alternatives,  
- le degré de dépendance entre choix au sein d’un même nœud.

### ✔ Probabilités prédictives :
Modélisation de la décision probable d’un individu selon ses caractéristiques.

Ces résultats permettent d’identifier clairement quels facteurs *augmentent* ou *réduisent* la probabilité qu’un individu :

- change de zone de travail,  
- reste dans son département,  
- effectue des déplacements longs.

---

##  8. Installation et exécution

###  Prérequis

```bash
git clone https://github.com/Midou94f/Projet-INSEE-analyse-Mobilite-IDF
cd Projet-INSEE-analyse-Mobilite-IDF

python -m venv venv
source venv/bin/activate        # macOS / Linux
venv\Scripts\activate           # Windows

pip install -r requirements.txt
Lancer les analyses
Ouvrir les notebooks :

bash
Copier le code
jupyter lab
Puis exécuter dans l’ordre :

CodeM1SE.ipynb – Préparation et exploration

projet_nestedlogit_mehdi.ipynb – Modèle économétrique

CodeCelib.ipynb – Analyses complémentaires

9. Résultats principaux (résumé)
Le Nested Logit confirme une structure hiérarchique forte entre types de mobilité.

Les variables les plus significatives concernent :

l’éducation,

la catégorie socioprofessionnelle,

la distance domicile-travail,

le département de résidence.

Certaines populations présentent des probabilités de mobilité nettement supérieures, suggérant potentiellement des inégalités structurelles de mobilité.

10. Limites du modèle
Données issues d’un échantillon partiel

Absence de certaines variables clés (coût réel du transport, temps de trajet précis)

Structure des alternatives parfois contrainte par la disponibilité des données

11. Améliorations envisageables
Passage à un Mixed Logit (préférences hétérogènes)

Ajout de variables géographiques précises (distances calculées via API)

Modélisation dynamique (panel, trajectoires de mobilité)

Cartographie interactive des flux

12. Licence
Ce projet est diffusé sous licence MIT (voir fichier LICENSE).

13. Auteur
Projet réalisé par Romain Mehdi Fehri dans le cadre d’un travail universitaire en statistique et économétrie.
