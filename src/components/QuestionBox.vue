<template>
  <div class="question-wrapper">
    <b-jumbotron>
      <template slot="lead">
        {{ currentQuestion.question }}
      </template>
      <hr>
      <p>
        List of answers
      </p>
      <b-button variant="primary">Submit</b-button>
      <b-button variant="primary">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: {
    currentQuestion: Object
  }
}
</script>