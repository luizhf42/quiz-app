<template>
  <main>
    <Home @startTheGame="startTheGame" v-if="!gameHasStarted" />
    <Question
      v-if="gameHasStarted && quizData && !gameFinished"
      :quizData="this.quizData"
      @endGame="endGame"
    />
    <div v-if="gameFinished">cabo</div>
  </main>
</template>

<script>
import "./App.scss";
import axios from "./services/axios";
import Home from "./components/Home.vue";
import Question from "./components/Question.vue";

export default {
  name: "App",
  components: {
    Home,
    Question,
  },
  data() {
    return {
      gameHasStarted: false,
      gameFinished: false,
      quizData: undefined,
    };
  },
  methods: {
    startTheGame({ difficulty, categoryId, questions }) {
      this.makeRequest(difficulty, categoryId, questions);
      this.gameHasStarted = true;
    },
    endGame(correctAnswers) {
      this.correctAnswers = correctAnswers;
      this.gameFinished = true;
    },
    async makeRequest(difficulty, categoryId, questions) {
      try {
        const { data } = await axios.get(
          `api.php?amount=${questions}&category=${categoryId}&difficulty=${difficulty}&type=multiple`
        );

        this.quizData = data;
        console.log(data);
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>
