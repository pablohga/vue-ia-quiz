<template>

  <section class="start-screen container">
    <h2>Choose a toic to start testing your knoewledege</h2>
    <input v-model="topic" type="text" placeholder="Enter your topic...">
    <button @click="emit('start-quiz', topic)" :disabled="!topic">Start Quiz</button>
    <p>{{ topic }}</p>
  </section>


</template>

<script setup>
import { ref } from 'vue'

const emit = defineEmits(['start-quiz'])

  const topic = ref('')

</script>
