# Petit plan — Gestionnaire de tâches

Application Vue 3 pour noter les tâches du jour, les marquer comme terminées et suivre le nombre de tâches restantes. Les tâches sont conservées en mémoire pendant la session du navigateur.

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

- Ajouter une tâche et ignorer les saisies vides.
- Cocher une tâche terminée; son texte est alors barré.
- Supprimer une tâche.
- Calculer automatiquement le nombre de tâches restantes et la progression.

## Notions Vue utilisées

- `ref` rend réactifs le texte saisi et la liste des tâches.
- `v-model` relie le champ texte et les cases à cocher à ces données.
- `v-for` affiche une ligne pour chaque tâche.
- `:key` fournit à Vue un identifiant stable pour chaque ligne.
- `computed` recalcule le compteur des tâches non terminées lorsque la liste change.
- `@submit.prevent` intercepte l’envoi du formulaire sans recharger la page.
- `@click` appelle la suppression de la tâche sélectionnée.

## Captures

- [Deux tâches et leur compteur](captures/taches-en-cours.png)
- [Tâche terminée et compteur actualisé](captures/tache-terminee.png)
