# Eli Lilly en France — Économie du cycle de vie d'un portefeuille de médicaments (2016–2025)

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-2.x-150458?logo=pandas&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-1.x-8CAAE6?logo=scipy&logoColor=white)
![Statut](https://img.shields.io/badge/Statut-Complet-brightgreen)

> Projet data science réalisé dans le cadre d'une candidature au **Master DS2E**
> (Data Science pour l'Économie et l'Entreprise) — Université de Strasbourg.

---

## Problématique

Comment le chiffre d'affaires remboursé d'un laboratoire pharmaceutique se
**renouvelle-t-il** dans le temps ? À partir des remboursements de l'Assurance
Maladie française, ce projet étudie le portefeuille d'**Eli Lilly** sur 2016–2025
et quantifie trois mécanismes de l'économie du médicament :

1. **La diffusion des innovations** — comment un nouveau médicament conquiert son marché
   (Trulicity, Verzenio, Jardiance).
2. **L'érosion générique** — comment un médicament mature perd sa valeur après
   l'arrivée des génériques (Cymbalta).
3. **Le déplacement du portefeuille** — vers des produits à prix unitaire élevé
   (oncologie, thérapies spécialisées).

---

## Les quatre produits : un cas d'école du cycle de vie

| Produit | Molécule | Aire thérapeutique | Trajectoire 2016→2025 | Phase du cycle |
|---------|----------|--------------------|-----------------------|----------------|
| **Trulicity** | Dulaglutide | Diabète (GLP-1) | 24 → 300 M€ | Croissance → maturité |
| **Verzenio** | Abémaciclib | Oncologie (cancer du sein) | 0 → 225 M€ | Lancement → croissance |
| **Jardiance** | Empagliflozine | Diabète / cardiovasculaire | 0 → 142 M€ | Lancement |
| **Cymbalta** | Duloxétine | Antidépresseur | 12 → 0,5 M€ | Déclin post-générique |

---

## Données

| Paramètre | Détail |
|-----------|--------|
| **Source** | [Open Medic — Assurance Maladie / AMELI](https://assurance-maladie.ameli.fr/etudes-et-donnees/open-medic-depenses-medicaments) |
| **Période** | 2016 – 2025 |
| **Périmètre** | Produits Eli Lilly identifiés par CIP13, enrichis (nom commercial, molécule, catégorie) |
| **Volume** | 20 130 lignes (médicament × âge × sexe × région × année) |
| **Fichier** | `data/eli_lilly.csv` — versionné dans le repo (projet reproductible en un `git clone`) |

### Variables principales
`nom_lilly`, `molecule`, `categorie` · `boites` (nombre de boîtes) · `rem` (montant remboursé, €) ·
`bse` (base de remboursement, €) · `age`, `sexe`, `ben_reg` (région) · `top_gen` (statut générique) · `annee`

---

## Notebooks

| Notebook | Contenu | Méthodes |
|----------|---------|----------|
| `00_extraction.ipynb` | Provenance : reproduit l'extraction Lilly depuis Open Medic brut et valide `data/eli_lilly.csv` | Traçabilité, contrôle de reproductibilité |
| `01_portefeuille.ipynb` | Cycle de vie, coût par boîte, décomposition prix × volume, composition du portefeuille | Décomposition économique, indices |
| `02_effet_generique.ipynb` | Autopsie du déclin de Cymbalta après l'arrivée des génériques | Analyse avant/après, contrefactuel |
| `03_diffusion_innovation.ipynb` | Ajustement de courbes de diffusion (logistique / Bass), prévision 2026–2027 | Estimation non linéaire, validation temporelle |
| `04_profils_patients.ipynb` | Profils âge × sexe × région × prescripteur par produit | Analyse démographique, croisements |

**Ordre d'exécution :** `00` (optionnel, nécessite les fichiers Open Medic bruts) → `01` → `02` → `03` → `04`

`00_extraction.ipynb` est optionnel : `data/eli_lilly.csv` est déjà versionné dans
le repo, donc `01` à `04` fonctionnent sans lui. Il sert à prouver que le fichier
est reproductible depuis les données sources (identification par mot-clé dans le
nom commercial CIP13), et s'arrête proprement si les fichiers bruts ne sont pas
présents sur la machine.

---

## Limites assumées

- **Périmètre remboursement, pas ventes** : Open Medic couvre les médicaments
  remboursés en ville, hors hôpital et hors part non remboursée. Ce n'est pas le
  chiffre d'affaires réel du laboratoire.
- **Mounjaro et Zepbound (tirzépatide) absents** : non remboursés en France sur
  la période étudiée. Le plateau apparent de Trulicity peut refléter une
  cannibalisation par ce nouveau produit, invisible ici.
- **Jardiance co-commercialisé** avec Boehringer Ingelheim : les remboursements ne
  sont pas attribuables à 100 % à Lilly.
- **Génériques concurrents non inclus** : le fichier ne contient que les produits
  Lilly. Le déclin de Cymbalta est mesuré côté princeps ; le volume capté par les
  génériques de duloxétine n'est pas dans ce fichier.
- **Code région 5 non résolu avec certitude** : le libellé « Mayotte »
  initialement associé au code `ben_reg = 5` s'est révélé incohérent avec les
  volumes observés (voir notebook 04). Il est renommé « DOM/TOM agrégés » par
  prudence ; à vérifier auprès de la documentation officielle Open Medic si
  besoin de précision.

---

## Installation

```bash
git clone https://github.com/<votre-username>/projet_lilly.git
cd projet_lilly
pip install -r requirements.txt
```

Les données sont incluses dans le repo — lancer directement les notebooks dans l'ordre.

---

*Données : Open Medic / AMELI — Assurance Maladie française.*
*Projet réalisé dans le cadre d'une candidature au Master DS2E.*
