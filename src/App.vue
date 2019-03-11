<template>
  <div id="app">

    <div v-if="questions.length - questionIndex > 0">
      <ul>
        <li>
          <template>
            <p>Question {{questionIndex + 1}}: {{questions[questionIndex]}}</p>
            <!-- <p>Correct Answer: {{correctAnswer[questionIndex]}}</p> -->
          </template>
          <button v-for="answers in randomList(questionsAnswers[questionIndex])"
                  :value = answers
                  @click="checkAnswer($event)"
                  type="button">
          {{answers}}
          </button>
          <br></br>
        </li>
      </ul>
      <p>{{questionIndex + 1}} / {{questions.length}}</p>
    </div>

    <div v-else>
      <p>quiz is finished</p>
      <p>You scored {{score}} out of {{questions.length}} points</p>
      <button @click="questionIndex = 0">restart quiz</button>
    </div>

  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      questions: [],
      questionsAnswers: [],
      correctAnswer: [],
      questionIndex: 0,
      score: 0
    }
  },
  created(){
    let i = 0
    let vm = this
    fetch('https://opentdb.com/api.php?amount=10&category=9&difficulty=easy&type=multiple')
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
  methods: {
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
      console.log(this.correctAnswer[this.questionIndex])

      if (buttonValue === this.correctAnswer[this.questionIndex]) {
        this.score += 1
      }

      this.questionIndex += 1
      
    } 
  }
}
  
</script>

<style>

</style>






