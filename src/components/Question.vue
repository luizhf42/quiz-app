<template>
  <section class="question">
    <h1>
      <span>{{ questionNumber }}</span
      >. <span v-html="questionText"></span>
    </h1>
    <p>Time left &#039;</p>

    <div class="answers">
      <div class="answer">
        {{ question1 }}
      </div>
      <div class="answer">
        {{ question2 }}
      </div>
      <div class="answer">
        {{ question3 }}
      </div>
      <div class="answer">
        {{ question4 }}
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: "Question",
  props: {
    quizData: Object,
  },
  data() {
    return {
      questionNumber: 1,
      index: 0,
      answers: [],
      questionText: "",
      question1: "",
      question2: "",
      question3: "",
      question4: "",
    };
  },
  mounted() {
    const questions = this.quizData.results;
    const index = this.index;

    // Put all the answers in an array
    this.answers.push(questions[index].correct_answer);
    questions[index].incorrect_answers.forEach((answer) => {
      this.answers.push(answer);
    });

    this.questionText = questions[index].question;
    this.question1 = this.getRandomAnswer();
    this.question2 = this.getRandomAnswer();
    this.question3 = this.getRandomAnswer();
    this.question4 = this.getRandomAnswer();
  },
  methods: {
    getRandomAnswer() {
      const randomNumber = Math.floor(Math.random() * this.answers.length);
      let answer = this.answers.splice(randomNumber, 1)[0];

      return answer;
    },
  },
};
</script>

<style lang="scss" scoped>
</style>