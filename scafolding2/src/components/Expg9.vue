<script setup>
import { ref } from 'vue'

// Definim una variable reactiva
const contador = ref(0)

// Funció que modifica el valor
const incrementar = () => {
  contador.value++
}
</script>
<template>
  <div>
    <p>El comptador és: {{ contador }}</p>
    <button @click="incrementar">Incrementar</button>
  </div>
</template>