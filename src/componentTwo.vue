<template>
  <div>
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
</template>

<script type="text/javascript">

  export default {
    props: {
      questionsAnswers: Array,
      questions: Array,
      questionIndex: Number,
      correctAnswer: Array,
      score: Number
    },
    methods: {
      checkAnswer: function(e) {
        console.log('questions.length: ' + this.questions.length)
        console.log('questionIndex: ' + this.questionIndex)
        const buttonValue = e.target.value;

        console.log(buttonValue)
        console.log(this.correctAnswer[this.questionIndex - 1])

        if (buttonValue === this.correctAnswer[this.questionIndex - 1]) {
          this.score += 1
        }
        this.questionIndex += 1
        this.$emit('questionIndexBackToParent', this.questionIndex); 
        
      },
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
      }
    }
  }
  
</script>

<style type="text/css">
  
</style>


