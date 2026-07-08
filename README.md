# pronogo-machine

**Chef d'orchestre public de la machine à contenu PronoGo.**

Ce dépôt ne contient **que** des workflows GitHub Actions d'orchestration. Il
existe pour une seule raison : faire tourner les routines automatiques
**gratuitement** (les Actions sont gratuites et illimitées sur un dépôt public),
indépendamment de la facturation du dépôt applicatif privé.

## Ce qu'il y a ici — et rien d'autre

- `.github/workflows/*.yml` — les orchestrateurs (prono du jour, menu de
  validation, mail, métriques, publication, refresh token).
- `.github/workflows/guard.yml` — la **règle bloquante** (voir ci-dessous).
- Ce `README.md` et la config `.gitleaks.toml`.

Les **scripts** (Python/JS), les **données** (posts, calendrier, métriques,
état) et **tout le code de l'application** vivent dans le dépôt **privé**
`fc-pronox`. Les workflows d'ici vont l'y chercher au moment de tourner (via un
token à portée minimale) et y réécrivent l'état.

## Règle bloquante — impossible à dévier

`guard.yml` s'exécute en premier sur chaque push / PR et **fait échouer** le run
si quoi que ce soit d'autre qu'un workflow, ce README ou la config apparaît :
liste blanche stricte + détection de secrets (gitleaks) + motifs interdits
(`lib/`, `functions/`, `*.dart`, `*.env`, clés, credentials…).

La branche `main` est protégée : un push qui viole la règle est **rejeté par
GitHub**, y compris pour un administrateur. Ce n'est pas une convention, c'est un
verrou.

> ⚠️ Les logs d'un dépôt public sont lisibles par tous. Aucun workflow ici ne
> doit afficher de contenu de fichier ni de valeur de secret — uniquement des
> compteurs et des noms de fichiers.
