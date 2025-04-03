<template>
  <section class= "quiz container">
    <h2>Quiz Comonente</h2>

    <div class="question">
      <h3>{{ props.questions[currentQuestion].question }}</h3>
    </div>

    <div class="answers">
      <button 
        class="answer"
        :class="{active: answer === selecedOption}"
        v-for="answer in shuffleOptions"
        :key="answer"
        @click = "selecedOption = answer"
      >
      {{ answer }}
    
    </button>
    </div>

    <button v-if="selecedOption" @click="subnmitAnswer">Send</button>

  </section>
</template>

<script setup>
import { ref, computed } from "vue"

const props = defineProps(['questions'])

const currentQuestion =ref(0)
const selecedOption = ref(null)

const shuffleOptions = computed(() => {
  let options = [...props.questions[currentQuestion.value].incorrect_answers]
  options.splice(Math.round((Math.random() * options.length)) , 0, props.questions[currentQuestion.value].correct_answer)
  return options
})

const subnmitAnswer = () =>{
  currentQuestion.value += 1
}

console.log(props.questions)
</script>

<style lang="scss" scoped>



</style>