<template>
  <div class="container text-center pt-5 pb-5">
    <div class="quiz-container">
      <div class="quiz-header mb-4">
        <div class="logo w-25 mx-auto">
          <img
            src="https://static.cdn.printful.com/static/images/layout/logo-icon.svg"
            alt=""
          />
        </div>
        <div class="heading">
          <h5>Printful Bits Quiz App</h5>
        </div>
      </div>
      <Introduction v-if="!isQuizStarted" @start-quiz="quizStarted" />
      <QuizBox
        v-if="isQuizStarted && !isQuizFinished"
        :username="username"
        :quizId="quizId"
        @results="quizFinished"
      />
      <ResultBox
        v-if="isQuizFinished"
        :username="username"
        :correctAnswers="correctAnswers"
        :totalQuestions="totalQuestions"
      />
    </div>
  </div>
</template>

<script>
import Introduction from "./components/Introduction.vue";
import QuizBox from "./components/QuizBox.vue";
import ResultBox from "./components/ResultBox.vue";
export default {
  name: "App",
  components: {
    Introduction,
    QuizBox,
    ResultBox,
  },
  data() {
    return {
      username: "",
      quizId: "",
      correctAnswers: 0,
      totalQuestions: 0,
      isQuizFinished: false,
      isQuizStarted: false,
    };
  },
  methods: {
    quizStarted(quizId, username) {
      this.isQuizStarted = true;
      this.username = username;
      this.quizId = quizId;
    },
    quizFinished(correctQuestions, totalQuestions) {
      this.isQuizFinished = true;
      this.quizStarted = false;
      this.correctAnswers = correctQuestions;
      this.totalQuestions = totalQuestions;
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap");

#app {
  font-family: "Roboto", sans-serif;
}
.quiz-container {
  background-color: #ecf0f1;
  padding: 1rem;
  -webkit-box-shadow: 10px 10px 10px 10px rgba(161, 170, 176, 1);
  -moz-box-shadow: 10px 10px 10px 10px rgba(161, 170, 176, 1);
  box-shadow: 10px 10px 10px 10px rgba(161, 170, 176, 1);
  border-radius: 5px;
}

.logo {
  min-width: 150px;
}

.button,
.button:active,
.button:hover {
  background-color: #00b5ac;
  border: #00b5ac;
  color: #495057;
  padding: 0.5rem 1rem;
  border: 1px solid rgba(0, 0, 0, 0.125);
}

.button:hover {
  box-shadow: 0 2px 4px 0 rgb(0 0 0 / 25%);
}

@media (min-width: 760px) {
  .quiz-container {
    min-width: 680px;
  }
}
</style>
