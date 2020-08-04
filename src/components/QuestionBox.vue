<template>
  <div class="QuestionBoxContainer">
    <b-jumbotron lead="Bootstrap v4 Components for Vue.js 2">
      <!-- <template v-slot:lead>{{currQuestion.question}}</template> -->
      <template v-slot:lead><span v-html="currQuestion.question"></span></template>
      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item v-for="(answer,index) in shuffledAnswers" 
        v-bind:key="index"
        @click.prevent="selectAnswer(index)"
        v-bind:class="answerClass(index)"
        >
          <span v-html="answer"></span>
          </b-list-group-item>
      </b-list-group>
      <!-- <b-button variant="secondary"
      v-bind:disabled="index<1"
      @click="previous"
      >Back{{index}}</b-button> -->

      <b-button variant="primary" 
      v-on:click="submitAnswer"
      v-bind:disabled="selectedIndex===null || answered"
      >
      Submit</b-button>
      <b-button variant="success" href="#" @click="next">Next</b-button>

    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash'
export default {
  //props is data passed from other component but data() is local
  props: {
    currQuestion: Object,
    next: Function,
    increment:Function,
    previous: Function,
    index: Number
  },
  data: function(){
    return{
      selectedIndex: null,
      shuffledAnswers:[],
      correctIndex: null,
      answered: false,
    }
  },
  watch: {
    currQuestion: {
      immediate: true,
      handler(){
      this.selectedIndex=null
      this.answered=false
      this.shuffleAnswers()
      }
    }
  },
  computed: {
    answers() {
      let answers = [...this.currQuestion.incorrect_answers]
      answers.push(this.currQuestion.correct_answer)
      return answers
    }
  },
  methods:{
    shuffleAnswers(){
      let answers = [...this.currQuestion.incorrect_answers,
      this.currQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.currQuestion.correct_answer)
      console.log(this.correctIndex)
    },
    selectAnswer(index) {
      this.selectedIndex = index
    },
    submitAnswer(){
      let isCorrect = false
      if(this.selectedIndex===this.correctIndex){
        isCorrect=true
      }
      this.answered=true
      this.increment(isCorrect)
    },
    answerClass(index){
      let answerClass = ''
      if(!this.answered && this.selectedIndex ===index){
        answerClass='selected'
      }
      else if(this.answered && this.correctIndex===index){
        answerClass='correct'
      }
      else if(this.answered && this.selectedIndex===index && this.correctIndex!==this.SelectedIndex){
        answerClass ='incorrect'
      }
      return answerClass
    }
  }
};
</script>
<style scoped>
  .list-group{
    margin-bottom: 15px;
  }
  .list-group-item:hover{
    cursor: pointer;
    background-color: lightgray;
  }
  .btn{
    margin: 0 20px;
  }
  .QuestionBoxContainer{
    margin-top: 30px;
  }
  .selected{
    background-color: lightblue;
  }
  .correct{
    background-color: lightgreen;
  }
  .incorrect{
    background-color: red;
  }
</style>