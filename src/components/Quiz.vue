<template>
  <section class= "quiz container">
    <h2>Quiz Comonente</h2>

    <div class="question">
      <h3>{{ props.questions[1].question }}</h3>
    </div>
  </section>
</template>

<script setup>
const props = defineProps(['questions'])
console.log(props.questions)
</script>

<style lang="scss" scoped>



</style>