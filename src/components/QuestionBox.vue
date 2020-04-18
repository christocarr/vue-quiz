<template>
  <div class="question-wrapper">
    <b-jumbotron>
      <template slot="lead">
        {{ currentQuestion.question }}
      </template>
      <hr />
      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click="answerSelect(index)"
          :class="[selectedAnswer === index ? 'selected-answer' : '']"
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
import _ from 'lodash';
export default {
  props: {
    currentQuestion: Object,
    nextQuestion: Function,
  },
  data() {
    return {
      selectedAnswer: null,
      shuffledAnswers: [],
    };
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    },
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedAnswer = null;
        this.shuffleAnswers();
      },
    },
  },
  methods: {
    answerSelect(index) {
      this.selectedAnswer = index;
    },
    shuffleAnswers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      this.shuffledAnswers = _.shuffle(answers);
    },
  },
};
</script>

<style scoped>
.list-group {
  margin-bottom: 1rem;
}
.list-group-item:hover {
  cursor: pointer;
  background-color: #7d9da8;
  color: white;
}
.btn {
  margin: 0 0.5rem;
}

.selected-answer {
  background-color: lightblue;
}
.correct-answer {
  background-color: lightgreen;
}
.incorrect-answer {
  background-color: salmon;
}
</style>
