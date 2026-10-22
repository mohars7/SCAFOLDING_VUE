<script setup>
import { ref, reactive, computed } from 'vue'

const tasques = ref([
  { id: 1, titol: 'Tasca 1', completada: true, data: new Date().toLocaleDateString() },
  { id: 2, titol: 'Tasca 2', completada: false, data: new Date().toLocaleDateString() },
  { id: 3, titol: 'Tasca 3', completada: false, data: new Date().toLocaleDateString() },
])
const novaTasca = ref('')
const error = ref(false)
let seguentId = 4

const filtre = reactive({
  nomesPendents: false,
})

const tasquesVisibles = computed(() => {
  if (filtre.nomesPendents) {
    return tasques.value.filter((t) => !t.completada)
  }
  return tasques.value
})

const total = computed(() => tasques.value.length)
const pendents = computed(() => tasques.value.filter((t) => !t.completada).length)

const afegirTasca = () => {
  const titol = novaTasca.value.trim()
  if (titol === '') {
    error.value = true
    return
  }
  error.value = false
  tasques.value.push({
    id: seguentId++,
    titol: titol,
    completada: false,
    data: new Date().toLocaleDateString(),
  })
  novaTasca.value = ''
}

const canviarEstat = (tasca) => {
  tasca.completada = !tasca.completada
}

const eliminarTasca = (id) => {
  tasques.value = tasques.value.filter((t) => t.id !== id)
}
</script>

<template>
  <div class="app">
    <h1>Gestor de Tasques</h1>

    <div>
      <input
        v-model="novaTasca"
        placeholder="Escriu una nova tasca"
        @keyup.enter="afegirTasca"
      />
      <button @click="afegirTasca">Afegir</button>
    </div>
    <p v-show="error" class="error">El títol no pot estar buit</p>

    <label>
      <input type="checkbox" v-model="filtre.nomesPendents" />
      Mostra només pendents
    </label>

    <ul v-if="tasquesVisibles.length > 0">
      <li v-for="tasca in tasquesVisibles" :key="tasca.id">
        <span :class="{ completada: tasca.completada }">{{ tasca.titol }}</span>
        <small> ({{ tasca.data }})</small>
        <button @click="canviarEstat(tasca)">
          {{ tasca.completada ? 'Desmarcar' : 'Completar' }}
        </button>
        <button @click="eliminarTasca(tasca.id)">🗑️</button>
      </li>
    </ul>
    <p v-else>No hi ha tasques per mostrar</p>

    <p>Total: {{ total }} | Pendents: {{ pendents }}</p>
  </div>
</template>

<style scoped>
.app {
  grid-column: 1 / -1;
  text-align: center;
}
ul {
  list-style: none;
  padding: 0;
}
li {
  margin: 6px 0;
}
button {
  margin-left: 6px;
}
.completada {
  text-decoration: line-through;
  color: gray;
}
.error {
  color: red;
}
</style>