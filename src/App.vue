<script setup>
import { ref, computed } from 'vue'

//variabili reactive
const anno = ref(new Date().getFullYear())
const mese = ref(new Date().getMonth())

//Funzione: numero giorni del mese
function getGiorniNelMese(anno, mese) {
  return new Date(anno, mese + 1, 0).getDate()
}

//Funzione: giorno della settimana del primo giorno del mese
function getPrimoGiornoSettimana(anno, mese) {
  return new Date(anno, mese, 1).getDay()
}

//Array dei giorni da mostrare nel calendario (con spazi vuoti)
const giorniCalendario = computed(() => {

  const giorni = []
  const giorniMese = getGiorniNelMese(anno.value, mese.value);
  const offset = getPrimoGiornoSettimana(anno.value, mese.value)

  //Caselle vuote all'inizio
  for (let i = 0; i < offset; i++) {
    giorni.push(null)
  }

  //Giorni reali del mese
  for (let d = 1; d <= giorniMese; d++) {
    giorni.push(d)
  }

  return giorni
})

//Oggetto per memorizzare valori dei campi di testo per ogni giorno
const noteGiorni = ref({})

// Gestione input
function aggiornaNota(giorno, valore){
  noteGiorni.value[giorno] = valore
}

// Gestione click giorni
function selezionaGiorno(giorno) {
  if (giorno) alert(`Hai cliccato sul ${giorno}/${mese.value + 1}/${anno.value}`)
}

//Gestione click giorni
function annoPrecedente() {anno.value--}
function annoSuccessivo() {anno.value++}

//Frecce per cambiare mese
function mesePrecedente() {
  if (mese.value === 0) {
    mese.value = 11
    anno.value--
  } else {
    mese.value--
  }
}

function meseSuccessivo() {
  if ( mese.value === 11) {
    mese.value = 0
    anno.value ++
  } else {
    mese.value ++
  }   
}

//Array nomi mesi
const nomiMesi = ['Gennaio','Febbraio','Marzo','Aprile','Maggio','Giugno','Luglio','Agosto','Settembre','Ottobre','Novembre','Dicembre']
</script>

<template>
  <div>
    <!-- Controlli anno/mese -->
    <div class="flex justify-between mb-2">
      <button @click="annoPrecedente">&lt; Anno</button>
      <h2>{{ anno }}</h2>
      <button @click="annoSuccessivo">Anno &gt;</button>
    </div>

    <div class="flex justify-between mb-4">
      <button @click="mesePrecedente">&lt; Mese</button>
      <h3>{{ nomiMesi[mese] }}</h3>
      <button @click="meseSuccessivo">Mese &gt;</button>
    </div>

    <!-- Calendario -->
    <div class="grid grid-cols-7 gap-2">
      <div
        v-for='(giorno, i) in giorniCalendario'
        :key="i"
        class="border p-2 text-center"
        :class="{ 'bg-gray-200' : !giorno }"        
        >

        <div v-if="giorno">{{ giorno }}</div>
        <input
          v-if="giorno"
          type="text"
          v-model="noteGiorni[giorno]"
          placeholder="Nota"
          class="border p-1 w-full mt-1"
      </div>

    </div>
  </div>
</template>

<style scoped>
.grid { display: grid; grid-template-columns: repeat(7, 1fr); }
.flex { display: flex; justify-content: space-between; align-items: center; }
.mb-2 { margin-bottom: 0.5rem; }
.mb-4 { margin-bottom: 1rem; }
button { padding: 0.5rem 1rem; cursor: pointer; }
.border { border: 1px solid #ccc; }
.p-2 { padding: 0.5rem; }
.p-1 { padding: 0.25rem; }
.w-full { width: 100%; }
.bg-gray-200 { background-color: #eee; }
.mt-1 { margin-top: 0.25rem; }
</style>
