# imc-vue

This template should help get you started developing with Vue 3 in Vite.

## Recommended IDE Setup

[VS Code](https://code.visualstudio.com/) + [Vue (Official)](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur).

## Recommended Browser Setup

- Chromium-based browsers (Chrome, Edge, Brave, etc.):
  - [Vue.js devtools](https://chromewebstore.google.com/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd)
  - [Turn on Custom Object Formatter in Chrome DevTools](http://bit.ly/object-formatters)
# Petit plan — Gestionnaire de tâches

Une petite application Vue 3 pour noter les tâches du jour, les marquer comme terminées et suivre le nombre de tâches restantes. Les tâches sont conservées en mémoire pendant la session du navigateur.

## Installation et lancement

```sh
npm install
npm run dev
```

Pour vérifier les types et construire le projet :

```sh
npm run build
```

## Fonctions réalisées

- Ajouter une tâche en appuyant sur Entrée ou sur le bouton `+`.
- Ignorer les saisies vides et supprimer les espaces inutiles.
- Cocher une tâche terminée; son texte est alors barré.
- Supprimer une tâche de la liste.
- Calculer automatiquement le nombre de tâches restantes et la progression.

## Notions Vue utilisées

- `ref` rend réactifs le texte saisi et la liste des tâches.
- `v-model` relie le champ texte et les cases à cocher à ces données.
- `v-for` affiche une ligne pour chaque tâche.
- `:key` fournit à Vue un identifiant stable pour chaque ligne.
- `computed` recalcule le compteur des tâches non terminées lorsque la liste change.
- `@submit.prevent` intercepte l’envoi du formulaire sans recharger la page.
- `@click` appelle la suppression de la tâche sélectionnée.

## Captures à remettre

Placez dans `captures/` les captures réalisées dans le navigateur :

- `taches-en-cours.png` : au moins deux tâches et leur compteur.
- `tache-terminee.png` : une tâche cochée et barrée, avec le compteur mis à jour.
- `depot-github.png` : dépôt public ouvert dans le navigateur et adresse visible.
```sh
npm run build
```

## Captures à remettre

- Calcul valide : `captures/calcul-valide.png`
- Erreur de saisie : `captures/erreur-saisie.png`

## Compte rendu des difficultés rencontrées

La principale difficulté a été de gérer les saisies décimales au format français tout en empêchant les valeurs vides, non numériques ou hors limites de produire un résultat trompeur. Les champs acceptent donc la virgule et le point, puis affichent une erreur explicite avant tout calcul invalide. Le calcul de l’IMC reste un indicateur général et est présenté comme tel.
