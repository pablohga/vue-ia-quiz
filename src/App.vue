<script setup>
import StartScreen from './components/StartScreen.vue';
import { GoogleGenerativeAI, SchemaType } from "@google/generative-ai";
import Quiz from "./components/Quiz.vue"
import Loader from "./components/Loader.vue"
import Results from "./components/Resullts.vue";

import { ref } from 'vue'

const questions = ref('')
const status = ref('start')
const userAnswers = ref([])

const storeAnswer = (answer) => {
  userAnswers.value.push(answer)
}


const startQuiz = async (topic)=> {

  status.value = 'loading'

const genAI = new GoogleGenerativeAI("AIzaSyBDLBz8Fc4oNLA0REemz_2Z2AzJbcyFcqg");

const schema = {
  description: "Quiz question response format",
  type: SchemaType.OBJECT,
  properties: {
    response_code: {
      type: SchemaType.NUMBER,
      description: "Response code indicating success (0 means no error)",
      nullable: false,
    },
    results: {
      type: SchemaType.ARRAY,
      description: "List of quiz questions",
      items: {
        type: SchemaType.OBJECT,
        properties: {
          type: {
            type: SchemaType.STRING,
            description: "Type of question (multiple-choice or true/false)",
            nullable: false,
          },
          difficulty: {
            type: SchemaType.STRING,
            description: "Difficulty level of the question",
            nullable: false,
          },
          category: {
            type: SchemaType.STRING,
            description: "Category to which the question belongs",
            nullable: false,
          },
          question: {
            type: SchemaType.STRING,
            description: "The quiz question text",
            nullable: false,
          },
          correct_answer: {
            type: SchemaType.STRING,
            description: "The correct answer to the question",
            nullable: false,
          },
          incorrect_answers: {
            type: SchemaType.ARRAY,
            description: "A list of incorrect answers",
            items: {
              type: SchemaType.STRING,
            },
            nullable: false,
          },
        },
        required: [
          "type",
          "difficulty",
          "category",
          "question",
          "correct_answer",
          "incorrect_answers",
        ],
      },
    },
  },
  required: ["response_code", "results"],
};

const model = genAI.getGenerativeModel({
  model: "gemini-1.5-pro",
  generationConfig: {
    responseMimeType: "application/json",
    responseSchema: schema,
  },
});

const result = await model.generateContent(
  `
  Create 5 quiz questions about ${topic}
  Difficulty: Easy to Medium
  Type: Mutiple Choice
  `,
);
questions.value = JSON.parse(result.response.text());
status.value = 'ready'
console.log(questions.value)

}
</script>

<template>
  <div id="app">
    <header>
      <div class="container">
        <img src="./assets/logo.png" alt="Logo" class="logo">
        <h1>Quiz Generatos</h1>
      </div>
    </header>

    <StartScreen v-if="status == 'start'" @start-quiz="startQuiz"/> 
    
    <Loader v-if="status == 'loading'"/>

    <Quiz  v-if="status === 'ready'"  @end-quiz="status = 'finished'" @store-answer="storeAnswer" :questions="questions.results"/> 

    <Results v-if="status == 'finished'" :userAnswers="userAnswers"/>

  </div>
</template>


