<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <Header 
    v-bind:numCorrect="numCorrect"
    v-bind:numTotal="numTotal"
    />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox 
          v-if="questions.length!=0"
          v-bind:currQuestion="questions[index]"
          v-bind:next="next"
          v-bind:increment="increment"
          v-bind:index="index"
          v-bind:previous="previous"
          />
        </b-col>
      </b-row>
    </b-container>
    
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox
  },
  data(){
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0
    }
  },
  methods: {
    next() {
      this.index++
    },
    increment(isCorrect){
      if(isCorrect){
        this.numCorrect++
      }
      this.numTotal++
    },
    previous(){
      this.index--;
    }
  },
  mounted: function(){
    fetch('https://opentdb.com/api.php?amount=10&difficulty=medium&type=multiple',{
    method: 'get'
    })
    .then(response => response.json())
    .then(data => this.questions=data.results) 
    .catch(err=>console.error(err))
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
