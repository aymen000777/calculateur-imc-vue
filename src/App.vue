<script setup lang="ts">
import { computed, ref } from 'vue'

interface Tache {
  id: number
  libelle: string
  terminee: boolean
}

const nouvelleTache = ref('')
const taches = ref<Tache[]>([])
let prochainId = 0

const tachesNonTerminees = computed(() =>
  taches.value.filter((tache) => !tache.terminee).length,
)

function ajouterTache() {
  const texteNettoye = nouvelleTache.value.trim()
  if (!texteNettoye) return

  taches.value.push({
    id: prochainId++,
    libelle: texteNettoye,
    terminee: false,
  })
  nouvelleTache.value = ''
}

function supprimerTache(id: number) {
  taches.value = taches.value.filter((tache) => tache.id !== id)
}
</script>

<template>
  <div class="app-shell">
    <header class="topbar">
      <a class="brand" href="#top" aria-label="Petit plan, accueil">
        <span class="brand-mark" aria-hidden="true"><span></span><span></span><span></span></span>
        <span>petit plan<span class="brand-period">.</span></span>
      </a>
      <div class="topbar-note"><span class="status-dot"></span> Votre journée, à votre rythme</div>
    </header>

    <main id="top" class="page-content">
      <section class="intro" aria-labelledby="page-title">
        <p class="eyebrow"><span>01</span> CARNET DU QUOTIDIEN</p>
        <h1 id="page-title">Les choses avancent,<br /><em>une à la fois.</em></h1>
        <p class="intro-copy">Posez vos idées ici. Chaque petite tâche terminée compte.</p>
      </section>

      <section class="task-board" aria-label="Gestionnaire de tâches">
        <div class="task-panel">
          <div class="panel-heading">
            <div>
              <p class="step-label">VOTRE LISTE</p>
              <h2>À faire aujourd’hui</h2>
            </div>
            <span class="panel-index">JOUR / 01</span>
          </div>

          <form class="form-ajout" @submit.prevent="ajouterTache">
            <label class="sr-only" for="nouvelle-tache">Nouvelle tâche</label>
            <input
              id="nouvelle-tache"
              v-model="nouvelleTache"
              type="text"
              maxlength="120"
              placeholder="Ajouter une tâche…"
              autocomplete="off"
            />
            <button class="add-button" type="submit" aria-label="Ajouter la tâche">
              <span aria-hidden="true">+</span>
            </button>
          </form>

          <div class="list-heading">
            <span>MES TÂCHES</span>
            <span v-if="taches.length">{{ taches.length }} au total</span>
          </div>

          <p v-if="taches.length === 0" class="empty-state">
            <span class="empty-mark" aria-hidden="true">↳</span>
            Votre liste est encore vide.<br />Ajoutez une première tâche pour commencer.
          </p>

          <ul v-else class="liste-taches">
            <li
              v-for="tache in taches"
              :key="tache.id"
              class="task-item"
              :class="{ terminee: tache.terminee }"
            >
              <label class="task-label">
                <input v-model="tache.terminee" type="checkbox" />
                <span class="checkmark" aria-hidden="true"></span>
                <span class="task-name">{{ tache.libelle }}</span>
              </label>
              <button
                class="delete-button"
                type="button"
                :aria-label="`Supprimer ${tache.libelle}`"
                @click="supprimerTache(tache.id)"
              >
                Supprimer
              </button>
            </li>
          </ul>
        </div>

        <aside class="progress-panel" aria-live="polite">
          <div class="result-topline">
            <span>EN COURS</span>
            <span class="result-icon" aria-hidden="true">↗</span>
          </div>
          <p class="remaining-count">{{ tachesNonTerminees }}<span>/ {{ taches.length }}</span></p>
          <h2>{{ tachesNonTerminees === 1 ? 'tâche restante' : 'tâches restantes' }}</h2>
          <p class="progress-note">
            {{ taches.length === 0
              ? 'Un petit pas suffit pour démarrer.'
              : tachesNonTerminees === 0
                ? 'Tout est terminé. Belle avancée.'
                : 'Continuez à votre rythme, vous avancez.' }}
          </p>
          <div class="progress-track" role="progressbar" :aria-valuenow="taches.length - tachesNonTerminees" :aria-valuemax="taches.length || 1" aria-label="Tâches terminées">
            <span :style="{ width: `${taches.length ? ((taches.length - tachesNonTerminees) / taches.length) * 100 : 0}%` }"></span>
          </div>
          <p class="progress-caption">{{ taches.length - tachesNonTerminees }} terminée{{ taches.length - tachesNonTerminees > 1 ? 's' : '' }}</p>
        </aside>
      </section>

      <footer class="page-footer">
        <span>FAIRE DE LA PLACE À L’ESSENTIEL</span>
        <span class="footer-rule"></span>
        <span>{{ String(tachesNonTerminees).padStart(2, '0') }} À FAIRE</span>
      </footer>
    </main>
  </div>
</template>
