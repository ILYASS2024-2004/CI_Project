# TP3 : Pipeline CI Jenkins & Publication Nexus (Partie CI)

**Université Sidi Mohamed Ben Abdellah**  
**École Nationale des Sciences Appliquées de Fès (ENSA Fès)**  
**Filière ILIA / Semestre 5 — DevOps**

---

## 📌 Présentation du Projet
Ce projet constitue la première partie du TP3 DevOps : la mise en œuvre de la chaîne d'**Intégration Continue (CI)** avec **Jenkins**, couplée à un dépôt d'artefacts **Nexus Repository Manager**.

L'application est un module Python `calculator.py` accompagné de sa suite de tests unitaires `test_calculator.py`, d'une analyse de qualité statique de code avec `pylint`, de la génération d'un livrable zippé (`Release.zip`) et de son téléversement automatique vers Nexus.

---

## 📂 Structure du Répertoire
```
CI_Project/
├── calculator.py              # Module métier (opérations mathématiques de base)
├── test_calculator.py         # Suite de 5 tests unitaires Pytest
├── requirements.txt           # Dépendances (pytest, pylint)
├── Jenkinsfile                # Définition du pipeline Jenkins CI (déclaratif)
├── .gitignore                 # Fichiers ignorés par Git
└── README.md                  # Documentation du projet
```

---

## ⚙️ Détail du Pipeline Jenkins (`Jenkinsfile`)

Le pipeline déclaratif exécute les étapes suivantes :
1. **Nettoyage Workspace** : suppression des fichiers résiduels du workspace via `deleteDir()`.
2. **Clonage** : extraction de la branche `main` depuis le dépôt GitHub.
3. **Vérification Python** : affichage des versions de Python et de Pip (`python --version`, `python -m pip --version`).
4. **Installation des dépendances** : installation des packages listés dans `requirements.txt`.
5. **Analyse qualité du code (Pylint)** : évaluation de la conformité du code Python avec `python -m pylint calculator.py || exit /b 0` (score cible : 10.00/10).
6. **Tests unitaires** : validation fonctionnelle avec `python -m pytest --ignore=Release`.
7. **Création de l'artefact** : préparation du dossier `Release/`, copie des fichiers essentiels et compression en `Release.zip` via PowerShell `Compress-Archive`.
8. **Publication de l'artefact** : envoi de l'archive `Release.zip` sur le dépôt hébergé Nexus `python-release` via une requête HTTP PUT avec `curl`.

---

## 🚀 Exécution et Vérification Locale

```bash
# Installer les dépendances
pip install -r requirements.txt

# Lancer les tests unitaires (5 passed)
pytest -v

# Lancer l'analyse de qualité Pylint (Score: 10.00/10)
pylint calculator.py
```

---

## 🔄 Commandes Git

```bash
git remote add origin https://github.com/VOTRE_NOM/CI_Project.git
git branch -M main
git push -u origin main
```
