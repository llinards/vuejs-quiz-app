<template>
  <div>
    <h3 class="mb-3">{{ greetings }}</h3>
    <h4 class="mb-3">{{ questionTitle }}</h4>
    <div class="list-group">
      <div class="container">
        <div class="row mb-3">
          <div
            class="answer-box mb-2 rounded-3 col-md-6 col-sm-12"
            v-for="answer in allAnswersInCurrentQuestion"
            :key="answer.id"
          >
            <label
              :for="answer.id"
              class="list-group-item list-group-item-action answer"
              :class="{ 'answer-selected': selectedAnswer == answer.id }"
            >
              <input
                type="radio"
                :id="answer.id"
                :value="answer.id"
                @click="answerSelected(answer.id)"
                class="visually-hidden"
              />
              {{ answer.title }}
            </label>
          </div>
        </div>
        <div class="cta-buttons mb-5">
          <button
            v-show="
              selectedAnswer &&
              currentQuestionIndex < totalNumberOfQuestions - 1
            "
            @click="nextQuestion"
            class="btn mx-auto w-35 button"
          >
            Next Question
          </button>
          <button
            v-show="
              selectedAnswer &&
              currentQuestionIndex == totalNumberOfQuestions - 1
            "
            @click="nextQuestion"
            class="btn mx-auto w-35 button"
          >
            Finish Quiz
          </button>
        </div>
        <div class="progress mb-3">
          <div class="progress-bar" :style="{ width: progressBar }"></div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "QuizBox",
  props: {
    username: String,
    quizId: Number,
  },
  data() {
    return {
      currentQuestionIndex: 0,
      totalNumberOfQuestions: 0,
      correctAnswers: 0,
      questionTitle: "",
      allQuestionsInQuiz: [],
      allAnswersInCurrentQuestion: [],
      selectedAnswer: "",
      selectedAnswers: [],
    };
  },
  computed: {
    progressBar() {
      return (
        (this.currentQuestionIndex / this.totalNumberOfQuestions) * 100 + "%"
      );
    },
    greetings() {
      if (this.currentQuestionIndex == 0) {
        return "Good luck, " + this.username + "!";
      } else if (this.currentQuestionIndex == this.totalNumberOfQuestions - 1) {
        return "Last question, " + this.username + "!";
      }
    },
  },
  methods: {
    async nextQuestion() {
      this.selectedAnswers.push(this.selectedAnswer);
      await this.checkIfAnswerIsCorrect(this.selectedAnswers);
      this.currentQuestionIndex += 1;
      if (this.currentQuestionIndex == this.totalNumberOfQuestions) {
        this.$emit("results", this.correctAnswers, this.totalNumberOfQuestions);
      } else {
        this.questionTitle =
          this.allQuestionsInQuiz[this.currentQuestionIndex].title;
        await this.getAnswersInCurrentQuestion();
      }
    },
    async checkIfAnswerIsCorrect(answerId) {
      try {
        const response = await axios.get(
          "https://printful.com/test-quiz.php?action=submit",
          {
            params: {
              quizId: this.quizId,
              answers: answerId,
            },
          }
        );
        if (response.data.correct) {
          this.correctAnswers = await response.data.correct;
        }
        this.selectedAnswer = "";
        this.allAnswersInCurrentQuestion = [];
      } catch (error) {
        console.log(error.message);
      }
    },
    async getAllQuestionsInQuiz() {
      try {
        const response = await axios.get(
          "https://printful.com/test-quiz.php?action=questions",
          {
            params: {
              quizId: this.quizId,
            },
          }
        );
        this.allQuestionsInQuiz = await response.data;
        this.totalNumberOfQuestions = this.allQuestionsInQuiz.length;
        this.questionTitle =
          this.allQuestionsInQuiz[this.currentQuestionIndex].title;
        await this.getAnswersInCurrentQuestion();
      } catch (error) {
        console.log(error.message);
      }
    },
    async getAnswersInCurrentQuestion() {
      try {
        const response = await axios.get(
          "https://printful.com/test-quiz.php?action=answers",
          {
            params: {
              quizId: this.quizId,
              questionId: this.allQuestionsInQuiz[this.currentQuestionIndex].id,
            },
          }
        );
        this.allAnswersInCurrentQuestion = await response.data;
      } catch (error) {
        console.log(error.message);
      }
    },
    answerSelected(answerId) {
      this.selectedAnswer = answerId;
    },
  },
  created() {
    this.getAllQuestionsInQuiz();
  },
};
</script>
<style>
.answer {
  font-size: 1rem;
}
.answer:hover {
  cursor: pointer;
}

.answer-selected,
.answer-selected:hover {
  background-color: #00b5ac;
}

.progress {
  background-color: inherit;
  border-radius: 5px;
}

.progress-bar {
  background-color: #00b5ac;
}
</style>