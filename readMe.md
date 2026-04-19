# Privelet — Confidentialité Différentielle sur Histogrammes

Implémentation de l'algorithme **Privelet** pour la publication différentiellement privée d'histogrammes sur des données ordinales unidimensionnelles.

---

## Contexte

Privelet est un mécanisme de confidentialité différentielle optimisé pour les requêtes d'histogrammes. Il exploite la **transformée de Haar** pour décomposer un histogramme en coefficients hiérarchiques, puis injecte un bruit Laplacien calibré selon les poids WHaar — réduisant l'erreur globale par rapport à un mécanisme naïf.

---

## Structure du projet

```
├── core_work.ipynb         # Notebook principal
├── adult/
│   └── adult.data          # Jeu de données UCI Adult
├── rapport_privelet.pdf    # Rapport technique
└── README.md
```

---

## Prérequis

```bash
pip install numpy scipy matplotlib pandas seaborn
```

---

## Pipeline Privelet

```
Histogramme → Haar Wavelet Transform → Poids WHaar → Bruit Laplacien → Transformée Inverse → Histogramme bruité
```

### Fonctions principales

| Fonction | Rôle |
|---|---|
| `haar_wavelet_transform(data)` | Décomposition Haar récursive |
| `compute_whaar_weights(n)` | Calcul des poids par niveau |
| `add_laplace_noise(coeffs, ε)` | Injection du bruit calibré |
| `inverse_haar_wavelet_transform(coeffs)` | Reconstruction du signal |
| `privelet_transform(data, ε)` | Pipeline complet |

---

## Paramètre ε

| ε | Bruit | Confidentialité | Précision |
|---|---|---|---|
| 0.01 | Très fort | ★★★★★ | ★☆☆☆☆ |
| 0.1  | Fort      | ★★★★☆ | ★★☆☆☆ |
| 1    | Modéré    | ★★★☆☆ | ★★★★☆ |
| 10   | Faible    | ★★☆☆☆ | ★★★★★ |

---

## Évaluation

La qualité est mesurée par la **distance de Wasserstein** entre l'histogramme original et l'histogramme bruité reconstruit, sur 20 runs par valeur de ε.

> La distance de Wasserstein décroît avec ε — compromis fondamental : **petit ε → meilleure confidentialité mais moins de précision**, et inversement.

---

## Jeu de données

**UCI Adult Dataset** — distribution des niveaux d'éducation (16 catégories, ~48k individus après nettoyage).

---

## Auteur

Franck LAGOU — Projet SBD
