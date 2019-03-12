<template>
  <form id = "startQuiz" name = "quiz" class="box">
    <p>Kies het aantal vragen</p>
    <select v-model="amount" class="form-control" name="amount">
      <option v-for="amount in 20" :value="amount">{{amount}}</option>{{amount}}
    </select>
    <p>Kies een categorie</p>
    <select v-model="category">
        <option v-for="(item, key) in categories" :value="item">{{key}}</option>
    </select>
    <p>Kies de moeilijkheidsgraad</p>
    <select v-model="difficulty">
        <option value="easy">Makkelijk</option>
        <option value="medium">Gemiddeld</option>
        <option value="hard">Moeilijk</option>
    </select>
    <button @click="startQuiz">Start Quiz!</button>
  </form> 
</template>

<script type="text/javascript">

import {eventBus} from './main';

export default {
    data() {
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
    methods: {
        startQuiz: function(event) {
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
            // eventBus.$emit('passQuestions', {'questionIndex':this.questionIndex, 'questions':this.questions});
            this.$emit('passData', {
                'questionIndex':this.questionIndex, 
                'questions':this.questions,
                'questionsAnswers':this.questionsAnswers,
                'correctAnswer':this.correctAnswer
            });
        }
    },
    computed: {
        title() {
            return 'https://opentdb.com/api.php?amount=' + this.amount + '&category=' + this.category + '&difficulty=' + this.difficulty + '&type=multiple'
        }
    }
}

    

</script>

<style type="text/css">
    
</style>