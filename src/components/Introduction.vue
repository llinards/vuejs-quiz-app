<template>
  <div>
    <h3>Welcome to the quiz!</h3>
    <p>Please enter your name and select quiz of your choice.</p>
    <p v-if="isError" class="text-danger">
      You need to provide name or/and select quiz!
    </p>
    <div class="mb-3">
      <input
        type="text"
        class="form-control w-50 mx-auto"
        id="name"
        v-model="username"
      />
    </div>
    <div class="mb-3">
      <select class="form-select w-50 mx-auto" v-model="quizId">
        <option
          v-for="quiz in availableQuizzes"
          :key="quiz.id"
          :value="quiz.id"
        >
          {{ quiz.title }}
        </option>
      </select>
    </div>
    <div class="mb-3">
      <button @click="quizStarted" class="btn button w-35">Start Quiz</button>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "Introduction",
  data() {
    return {
      availableQuizzes: [],
      selectedQuizData: {},
      username: "",
      quizId: 0,
      isError: false,
    };
  },
  methods: {
    async getAllQuizzess() {
      try {
        const response = await axios.get(
          "https://printful.com/test-quiz.php?action=quizzes"
        );
        this.availableQuizzes = await response.data;
      } catch (error) {
        console.log(error.message);
      }
    },
    quizStarted() {
      if (this.username == "" || this.quizId == 0) {
        this.isError = true;
      } else {
        this.isError = false;
        this.$emit("start-quiz", this.quizId, this.username);
      }
    },
  },
  created() {
    this.getAllQuizzess();
  },
};
</script>