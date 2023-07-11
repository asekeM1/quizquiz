<script setup>
import Question from '../components/Question.vue'
import QuizHeader from './QuizHeader.vue';
import Result from '@/components/Result.vue'
import {useRoute} from 'vue-router';
import { ref, watch, computed } from 'vue';
import quizes from '../data/quizez.json'

const route = useRoute()
const quizId = parseInt(route.params.id);
const quiz = quizes.find(q => q.id === quizId) 
const currentQuestionIndex = ref(0)
const numberOfCorrectAnswers = ref(0)

const questionStatus = computed(() => `${currentQuestionIndex.value}/${quiz.questions.length}`)
const barPercentage = computed(() => `${currentQuestionIndex.value/quiz.questions.length * 100}%`)

const onOptionSelected = (isCorrect) =>{
    if(isCorrect){
        numberOfCorrectAnswers.value++;
    }

    if(quiz.questions.length - 1 === currentQuestionIndex.value){
        showResults.value = true
    }
    currentQuestionIndex.value++;
}

const showResults = ref(false)

</script>

<template>
    <div>
        <QuizHeader 
        :questionStatus="questionStatus"
        :barPercentage="barPercentage
        "/>        
        <div>
            <Question v-if="!showResults" :question="quiz.questions[currentQuestionIndex]" @selectOption="onOptionSelected" />
        </div>
        <Result 
            v-if="showResults"
            :quizQuestionLenght="quiz.questions.length"
            :numberOfCorrectAnswers="numberOfCorrectAnswers"
        />
    </div>
</template>