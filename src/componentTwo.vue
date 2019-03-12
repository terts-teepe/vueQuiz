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

import {eventBus} from './main';

export default {
  created() {
    eventBus.$on('passQuestions', function(questionIndex, questions) {
      console.log(questionIndex);
      console.log(questions);
    });
  }
}
  
</script>

<style type="text/css">
  
</style>