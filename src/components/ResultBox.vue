<template>
  <div>
    <h3 class="mb-3">{{ username }}, you successfully completed the quiz!</h3>
    <h4 class="mb-3">
      Your score is: <strong>{{ correctAnswers }}</strong
      >/<strong>{{ totalQuestions }}</strong>.
    </h4>
    <button @click="reloadPage" class="btn button w-35 rounded-3">Start over</button>
  </div>
</template>

<script>
export default {
  name: "ResultBox",
  props: {
    correctAnswers: Number,
    totalQuestions: Number,
    username: String,
  },
  data() {
    return {};
  },
  methods: {
    reloadPage() {
      window.location.reload();
    },
  },
};
</script>