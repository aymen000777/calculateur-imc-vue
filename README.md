# Repère — Calculateur d’IMC

Application Vue 3 en français qui estime l’indice de masse corporelle à partir du poids et de la taille. Les valeurs peuvent être saisies avec une virgule ou un point décimal; les données restent dans le navigateur.

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

- Calculer l’IMC avec la formule poids (kg) / taille (m)².
- Valider le poids (20 à 300 kg) et la taille (80 à 250 cm).
- Afficher une catégorie indicative et une échelle visuelle.
- Réinitialiser le formulaire et corriger une saisie invalide.

## Notions Vue utilisées

- `ref` stocke les champs, le résultat et le message d’erreur de façon réactive.
- `computed` convertit les valeurs saisies et détermine la catégorie de l’IMC.
- `v-model` synchronise les champs avec les valeurs réactives.
- `@submit.prevent` lance le calcul sans recharger la page.
- `@click` permet de réinitialiser le formulaire.
- `:class` et `:style` adaptent l’état visuel à l’erreur et au résultat.

## Captures

- `captures/calcul-valide.png` : résultat obtenu avec des mesures valides.
- `captures/erreur-saisie.png` : message affiché pour une valeur invalide.

L’IMC est un indicateur général et ne remplace pas l’avis d’un professionnel de santé.
