<template>
<h1>Esta opcion esta hecha con composition y tiene el modulo canvas:</h1>
<p> ademas ha sido modificado dentro del contenedor y tiene usuario root. no se deja modificar desde el host.</p>
<CounterComposition/>
</template>

<script setup>
import CounterComposition from './components/CounterComposition.vue'
</script>

<style>
h1{
  color:blue;
}
</style>
