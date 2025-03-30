<template>
  <section class= "quiz container">
    <h2>Quiz Comonente</h2>

    <div class="question">
      <h3>{{ props.questions[currentQuestion].question }}</h3>
    </div>

    <button @click="subnmitAnswer">Send</button>
  </section>
</template>

<script setup>
import { ref } from "vue"

const props = defineProps(['questions'])
const currentQuestion =ref(0)

const subnmitAnswer = () =>{
  currentQuestion.value += 1
}

console.log(props.questions)
</script>

<style lang="scss" scoped>



</style>