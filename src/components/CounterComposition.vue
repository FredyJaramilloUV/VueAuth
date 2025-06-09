<template>
  <h3>{{ counter }}</h3>
<button @click="addCounter" onkeydown.i="addCounter">Incrementar</button>
</template>

<script>
import { defineComponent, ref } from 'vue'
export default defineComponent({
  name: 'CounterComposition',
  setup () {
    const counter = ref(0)
    const addCounter = () => {
      counter.value++
    }
    return { counter, addCounter }
  }
})
</script>

<style>

</style>
