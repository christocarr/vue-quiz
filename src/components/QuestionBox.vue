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
          :class="answeredClass(index)"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>
      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedAnswer === null || isAnswered"
        >Submit</b-button
      >
      <b-button 
        @click="nextQuestion" 
        variant="primary"
        :disabled="isAnswered === false"
        >Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash';
export default {
  props: {
    currentQuestion: Object,
    nextQuestion: Function,
    increment: Function,
  },
  data() {
    return {
      selectedAnswer: null,
      correctIndex: null,
      shuffledAnswers: [],
      isAnswered: false,
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
        this.isAnswered = false;
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
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedAnswer === this.correctIndex) {
        isCorrect = true;
      }
      this.increment(isCorrect);
      this.isAnswered = true;
    },
    answeredClass(index) {
      let answerClass =
        !this.isAnswered && this.selectedAnswer === index
          ? 'selected-answer'
          : this.isAnswered && this.correctIndex === index
          ? 'correct-answer'
          : this.isAnswered &&
            this.selectedAnswer === index &&
            this.correctIndex !== index
          ? 'incorrect-answer'
          : '';
      return answerClass;
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

.btn:disabled {
  background-color: lightgrey;
  border: none;
  cursor: default;
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
