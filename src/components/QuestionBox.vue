<template>
  <div class="question-box-container">
    <b-jumbotron>

      <template slot="lead">
        {{ this.currentQuestion.question}}
      </template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item v-for="(answer, index) in answers" :key="index" @click="selectAnswer(index)"
          :class="answerClass(index)"
        > {{ answer}}</b-list-group-item>
       
      </b-list-group>

      <b-button
        variant= "primary"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
      >Submit</b-button>
      <b-button @click="next" variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template>
<script>
import _ from  'lodash'

export default {
  name: 'QuestionBox',
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data(){
    return {
      selectedIndex: null,
      shuffledAnswers: [],
      correctIndex:null,
      answered: false
    }
  },
  methods: {
    selectAnswer(value) {
      this.selectedIndex = value
    },
    submitAnswer() {
      let isCorrect = false

      if(this.selectedIndex == this.correctIndex ) {
        isCorrect = true
      }
      this.answered = true;
      this.increment(isCorrect)
    },
    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
      this.shuffledAnswers = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    answerClass(index){
      if(!this.answered && this.selectedIndex == index ) {
        return 'selected'
      } else if( this.answered && this.correctIndex === index) {
        return 'correct'
      } else if(this.answered && this.selectedIndex == index && this.correctIndex != index){
        return 'incorrect'
      }
      return ''
    }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)
      return answers
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler(){
        this.shuffleAnswers();
        this.selectedIndex = null
        this.answered = false
      }
    }
  
  }
}
</script>

<style scoped>
  .list-group {
    margin-bottom: 15px !important;
  }

  .list-group-item:hover{
    background: #EEE;
    cursor: pointer;
  }

  .btn{
    margin: 0px 5px;
  }

  .selected {
    background: blue;
  }

  .correct {
    background: green;
  }

  .incorrect {
    background: lightcoral;
  }
</style>