<template>
  <div class="grid" id="app">
    <div v-if="questionIndex === 0">
      <!--Get values from component-one:  -->
      <component-one @passData=
        "questionIndex = $event.questionIndex, 
        questions = $event.questions,
        questionsAnswers=$event.questionsAnswers,
        correctAnswer=$event.correctAnswer">
      </component-one>
    </div>
    <!-- Send the data that is reveived from componen one towards component two 
    and get the score and questionIndex back from component two to the parent. -->
    <div v-else-if="questions.length - questionIndex > -1">
      <component-two @questionIndexBackToParent=
                      "questionIndex = $event.questionIndex,
                       score = $event.score"
                     :questions="questions"
                     :questionsAnswers="questionsAnswers"
                     :correctAnswer="correctAnswer"
                     :questionIndex="questionIndex">
      </component-two>
    </div>

    <div v-else class="container">
      <div class="containerTwo">
        <img class="box imageOne" src="./assets/img/emma.png" id="emma"/>
        <div class="talk-bubble tri-right left-in round box">
          <div class="talktext">
            <p>You scored {{score}} out of {{questions.length}} points</p>
          </div>
        </div>
      </div>
      <button id='myButton' @click="restartQuiz">Play Again!</button>
    </div>

  </div>
</template>

<script>

import componentOne from './componentOne.vue';
import componentTwo from './componentTwo.vue';

export default {
  name: 'app',
  data () {
    return {
      questions: [],
      questionsAnswers: [],
      correctAnswer: [],
      questionIndex: 0,
      score: ''
    }
  },
  components: {
    'component-one': componentOne,
    'component-two': componentTwo
  },
  methods: {
    restartQuiz: function(){
      this.questionIndex = 0,
      this.questions = [],
      this.questionsAnswers = [],
      this.correctAnswer = [],
      this.score = 0
    }
  }
}
  
</script>

<style>
  @import './assets/styles.css';
</style>






