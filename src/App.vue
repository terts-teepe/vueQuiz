<template>
  <div id="app">
    <div v-if="questionIndex === 0">
      <component-one @passData=
      "questionIndex = $event.questionIndex, 
      questions = $event.questions,
      questionsAnswers=$event.questionsAnswers,
      correctAnswer=$event.correctAnswer">
      </component-one>
    </div>

    <div v-else-if="questions.length - questionIndex > -1">
      <ul>
        <li>
          <template>
            <p>Question {{questionIndex}}: {{questions[questionIndex - 1]}}</p>
          </template>
          <button v-for="answers in randomList(questionsAnswers[questionIndex - 1])"
                  :value = answers
                  @click="checkAnswer($event)"
                  type="button">
          {{answers}}
          </button>
          <br></br>
        </li>
      </ul>
      <p>{{questionIndex}} / {{questions.length}}</p>
    </div>

    <div v-else>
      <p>quiz is finished</p>
      <p>You scored {{score}} out of {{questions.length}} points</p>
      <button @click="restartQuiz">Play Again!</button>
    </div>

  </div>
</template>

<script>

import componentOne from './componentOne.vue';

export default {
  name: 'app',
  data () {
    return {
      questions: [],
      jsonString: '',
      questionsAnswers: [],
      correctAnswer: [],
      questionIndex: 0,
      score: 0,
      category: '',
      difficulty:'',
      amount: '',
      categories: {
        Algemenekennis: 9,
        Sport: 21,
        Aardrijkskunde: 22,
        Geschiedenis: 23,
        Muziek: 12,
        Televisie: 14
      }
    }
  },
  components: {
    'component-one': componentOne
  },
  methods: {
    //Function to shuffle the array which contains the correct and wrong answers
    randomList(array) {
        var currentIndex = array.length;
        var temporaryValue;
        var randomIndex;
        var myRandomizedList;

        // Clone the original array into myRandomizedList (shallow copy of array)
        myRandomizedList = array.slice(0)

        // Randomize elements within the myRandomizedList - the shallow copy of original array
        // While there remain elements to shuffle...
        while (0 !== currentIndex) {

            // Pick a remaining element...
            randomIndex = Math.floor(Math.random() * currentIndex);
            currentIndex -= 1;

            // And swap it with the current element.
            temporaryValue = myRandomizedList[currentIndex];
            myRandomizedList[currentIndex] = myRandomizedList[randomIndex];
            myRandomizedList[randomIndex] = temporaryValue;
        }

        // Return the new array that has been randomized
        return myRandomizedList;
    },
    checkAnswer: function(e) {
      const buttonValue = e.target.value;

      console.log(buttonValue)
      console.log(this.correctAnswer[this.questionIndex - 1])

      if (buttonValue === this.correctAnswer[this.questionIndex - 1]) {
        this.score += 1
      }

      this.questionIndex += 1
      
    },
    startQuiz: function() {
      console.log('check parent')
      this.questionIndex += 1
      let i = 0
      let vm = this
      fetch(vm.title)
        .then((response) => {
          return response.json().then((json) => {
            for (i=0; i<json.results.length; i++) {

              vm.questions.push(json.results[i].question)
              vm.correctAnswer.push(json.results[i].correct_answer)
              vm.questionsAnswers.push([json.results[i].correct_answer, json.results[i].incorrect_answers[0], json.results[i].incorrect_answers[1], json.results[i].incorrect_answers[2]])
              
            }
            console.log(vm.questions)
            console.log(vm.correctAnswer)
            console.log(vm.questionsAnswers)
        })
      }) 
    },
    restartQuiz: function(){
      this.questionIndex = 0,
      this.questions = [],
      this.questionsAnswers = [],
      this.correctAnswer = [],
      this.score = 0
    },
    onClickChild (value) {
      console.log(value) // someValue
    }
  },
  computed: {
    title() {
        return 'https://opentdb.com/api.php?amount=' + this.amount + '&category=' + this.category + '&difficulty=' + this.difficulty + '&type=multiple'
    }
  }
}
  
</script>

<style>

</style>






