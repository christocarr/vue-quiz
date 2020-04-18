<template>
  <div class="question-wrapper">
    <b-jumbotron>
      <template slot="lead">
        {{ currentQuestion.question }}
      </template>
      <hr>
      <b-list-group>
        <b-list-group-item v-for="(answer, index) in answers" :key="index" >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>
      <b-button variant="primary">Submit</b-button>
      <b-button @click="nextQuestion" variant="primary">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: {
    currentQuestion: Object,
    nextQuestion: Function
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)
      return answers
    }
  }
}
</script>