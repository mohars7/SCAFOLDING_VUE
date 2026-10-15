<script setup>
import { ref, computed } from 'vue'

const nombre = ref('Joan')

const nombreMayusculas = computed(() =>
  nombre.value.toUpperCase()
)
</script>

<template>
  <div>
    <p>Nombre: {{ nombre }}</p>
    <p>Nombre en mayúsculas: {{ nombreMayusculas }}</p>
  </div>
</template>