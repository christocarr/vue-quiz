<template>
  <div class="question-wrapper">
    <b-jumbotron>
      <template slot="lead">
        {{ currentQuestion.question }}
      </template>
      <hr />
      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in answers"
          :key="index"
          @click="answerSelect(index)"
        >
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
    nextQuestion: Function,
  },
  data() {
    return {
      selectedAnswer: null,
    };
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    },
  },
  methods: {
    answerSelect(index) {
      this.selectedAnswer = index;
    },
  },
};
</script>

<style scoped>
  .list-group {
    margin-bottom: 1rem;
  }
  .btn {
    margin: 0 0.5rem;
  }
</style>