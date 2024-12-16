<template>
  <ScoreBoard />
  <section class="score">
    Jogador <span>0</span> x <span>0</span> Computador
  </section>
  <template v-if="this.question">
    <h1 v-html="this.question"></h1>
    <template :key="index" v-for="(answer, index) in this.answers">
      <input
        :disabled="this.answerSubmitted"
        v-model="this.chosenAnswer"
        type="radio"
        name="options"
        :value="answer"
      />
      <label v-html="answer"></label><br />
    </template>
    <template v-if="!answerSubmitted">
      <button @click="submitAnswer()" class="send" type="button">
        Confirmar
      </button>
    </template>
  </template>

  <section class="result" v-if="this.answerSubmitted">
    <template v-if="this.chosenAnswer == this.correctAnswer">
      <h4
        v-html="
          ' &#9989; Parabéns, a resposta: ' +
          this.correctAnswer +
          ' Esta correta!'
        "
      />
    </template>
    <template v-else>
      <h4
        v-html="
          '&#10060; Que pena, a resposta está errada. A resposta correta é ' +
          this.correctAnswer +
          '.'
        "
      />
    </template>
    <button @click="getNewQuestion()" class="send" type="button">
      Próxima pergunta
    </button>
  </section>
</template>

<script>
import ScoreBoard from "@/components/ScoreBoard.vue";

export default {
  name: "App",
  components: {
    ScoreBoard,
  },
  data() {
    return {
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      chosenAnswer: undefined,
      answerSubmitted: false,
      winCount: 0,
      loseCount: 0,
    };
  },
  methods: {
    getNewQuestion() {
      this.getQuestion();
    },
    getQuestion() {
      this.answerSubmitted = false;
      this.chosenAnswer = undefined;
      this.question = undefined;
      this.axios
        .get("https://opentdb.com/api.php?amount=10&category=18&type=multiple")
        .then((response) => {
          this.question = response.data.results[0].question;
          this.incorrectAnswers = response.data.results[0].incorrect_answers;
          this.correctAnswer = response.data.results[0].correct_answer;
          console.log(response.data);
        });
    },
    submitAnswer() {
      if (!this.chosenAnswer) {
        return alert("Escolha uma das opções!");
      }
      this.answerSubmitted = true;
      if (this.chosenAnswer !== this.correctAnswer) {
        alert(`Você errou!`);
        return;
      }
      alert("Você acertou");
    },
  },
  computed: {
    answers() {
      var answers = [...this.incorrectAnswers];
      answers.splice(
        Math.round(Math.random()) * answers.length,
        0,
        this.correctAnswer
      );
      return answers;
    },
  },
  created() {
    this.getQuestion();
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  margin-top: 60px;
  input[type="radio"] {
    margin: 12px 4px;
  }
  button.send {
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    padding: 0 16px;
    color: white;
    background-color: blue;
    border: 1px solid blue;
    cursor: pointer;
  }
}
</style>
