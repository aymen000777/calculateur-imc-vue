<script setup lang="ts">
import { computed, ref } from 'vue'

const weight = ref('')
const height = ref('')
const error = ref('')
const bmi = ref<number | null>(null)

const parsedWeight = computed(() => Number(weight.value.trim().replace(',', '.')))
const parsedHeight = computed(() => Number(height.value.trim().replace(',', '.')))

const category = computed(() => {
  if (bmi.value === null) return null
  if (bmi.value < 18.5) return { name: 'Insuffisance pondérale', tone: 'low' }
  if (bmi.value < 25) return { name: 'Corpulence normale', tone: 'normal' }
  if (bmi.value < 30) return { name: 'Surpoids', tone: 'high' }
  return { name: 'Obésité', tone: 'high' }
})

function calculateBmi() {
  error.value = ''
  bmi.value = null

  const weightIsValid = Number.isFinite(parsedWeight.value) && parsedWeight.value >= 20 && parsedWeight.value <= 300
  const heightIsValid = Number.isFinite(parsedHeight.value) && parsedHeight.value >= 80 && parsedHeight.value <= 250

  if (!weightIsValid && !heightIsValid) {
    error.value = 'Saisissez un poids entre 20 et 300 kg et une taille entre 80 et 250 cm.'
    return
  }
  if (!weightIsValid) {
    error.value = 'Le poids doit être compris entre 20 et 300 kg.'
    return
  }
  if (!heightIsValid) {
    error.value = 'La taille doit être comprise entre 80 et 250 cm.'
    return
  }

  const heightInMeters = parsedHeight.value / 100
  bmi.value = parsedWeight.value / (heightInMeters * heightInMeters)
}

function resetForm() {
  weight.value = ''
  height.value = ''
  bmi.value = null
  error.value = ''
}
</script>

<template>
  <div class="app-shell">
    <header class="topbar">
      <a class="brand" href="#top" aria-label="Repère, accueil">
        <span class="brand-mark" aria-hidden="true"><span></span><span></span><span></span></span>
        <span>repère<span class="brand-period">.</span></span>
      </a>
      <div class="topbar-note"><span class="status-dot"></span> Votre santé, en perspective</div>
    </header>

    <main id="top" class="page-content">
      <section class="intro" aria-labelledby="page-title">
        <p class="eyebrow"><span>01</span> OUTIL DE MESURE PERSONNELLE</p>
        <h1 id="page-title">Un repère simple.<br /><em>Pas une étiquette.</em></h1>
        <p class="intro-copy">Estimez votre indice de masse corporelle et prenez un instant pour comprendre ce qu’il indique.</p>
      </section>

      <section class="calculator" aria-label="Calculateur d’indice de masse corporelle">
        <div class="form-panel">
          <div class="panel-heading">
            <div>
              <p class="step-label">VOTRE MESURE</p>
              <h2>Quelques chiffres</h2>
            </div>
            <span class="panel-index">IMC / 01</span>
          </div>

          <form class="measure-form" novalidate @submit.prevent="calculateBmi" @input="error = ''; bmi = null">
            <label class="field-label" for="weight">Votre poids</label>
            <div class="input-wrap" :class="{ invalid: error && (!Number.isFinite(parsedWeight) || parsedWeight < 20 || parsedWeight > 300) }">
              <input id="weight" v-model="weight" type="text" inputmode="decimal" autocomplete="off" placeholder="Ex. 68" :aria-invalid="Boolean(error && (!Number.isFinite(parsedWeight) || parsedWeight < 20 || parsedWeight > 300))" aria-describedby="weight-unit" />
              <span id="weight-unit" class="unit">kg</span>
            </div>

            <label class="field-label second-field" for="height">Votre taille</label>
            <div class="input-wrap" :class="{ invalid: error && (!Number.isFinite(parsedHeight) || parsedHeight < 80 || parsedHeight > 250) }">
              <input id="height" v-model="height" type="text" inputmode="decimal" autocomplete="off" placeholder="Ex. 172" :aria-invalid="Boolean(error && (!Number.isFinite(parsedHeight) || parsedHeight < 80 || parsedHeight > 250))" aria-describedby="height-unit" />
              <span id="height-unit" class="unit">cm</span>
            </div>

            <p v-if="error" class="error-message" role="alert"><span aria-hidden="true">!</span>{{ error }}</p>

            <button class="calculate-button" type="submit">
              Calculer mon IMC
              <span aria-hidden="true">↗</span>
            </button>
            <p class="form-hint">Vos données restent dans votre navigateur.</p>
          </form>
        </div>

        <div class="result-panel" :class="{ 'has-result': bmi !== null, 'has-error': Boolean(error) }" aria-live="polite">
          <div class="result-topline">
            <span>VOTRE RÉSULTAT</span>
            <span class="result-icon" aria-hidden="true">↘</span>
          </div>

          <template v-if="bmi !== null && category">
            <p class="result-number">{{ bmi.toFixed(1).replace('.', ',') }}<span>IMC</span></p>
            <div class="category-line" :class="`tone-${category.tone}`">
              <span class="category-dot"></span>{{ category.name }}
            </div>
            <div class="scale" aria-label="Échelle indicative de l’IMC">
              <span class="scale-segment underweight"></span><span class="scale-segment healthy"></span><span class="scale-segment overweight"></span><span class="scale-segment obesity"></span>
              <span class="scale-marker" :style="{ left: `${Math.min(Math.max(((bmi - 15) / 25) * 100, 1), 99)}%` }"></span>
            </div>
            <div class="scale-labels"><span>15</span><span>18,5</span><span>25</span><span>30</span><span>40+</span></div>
            <p class="result-note">L’IMC est un indicateur général. Il ne remplace pas l’avis d’un professionnel de santé.</p>
            <button class="reset-button" type="button" @click="resetForm">Recommencer <span aria-hidden="true">↺</span></button>
          </template>

          <template v-else>
            <div class="empty-result">
              <div class="orbit" aria-hidden="true"><span></span><span></span><span></span><b>IMC</b></div>
              <h2>{{ error ? 'Vérifiez vos mesures' : 'Votre résultat\napparaîtra ici.' }}</h2>
              <p>{{ error ? 'Corrigez la valeur signalée puis relancez le calcul.' : 'Entrez votre poids et votre taille pour commencer.' }}</p>
            </div>
            <div class="range-key"><span><i class="key-dot key-green"></i>18,5–24,9</span><span>Repère adulte</span></div>
          </template>
        </div>
      </section>

      <footer class="page-footer">
        <span>UNE MESURE PARMI D’AUTRES</span>
        <span class="footer-rule"></span>
        <span>IMC = POIDS / TAILLE²</span>
      </footer>
    </main>
  </div>
</template>
