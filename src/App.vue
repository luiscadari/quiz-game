<template>
  <section class="score">
    Jogador <span>0</span> x <span>0</span> Computador
  </section>
  <h1>
    Which buzzword did Apple Inc. use to describe their removal of the headphone
    jack?
  </h1>
  <input type="radio" name="options" value="Innovation" /><label
    >Innovation</label
  >
  <br />
  <input type="radio" name="options" value="Revolution" /><label
    >Revolution</label
  >
  <br />
  <input type="radio" name="options" value="Courage" />
  <label>Courage</label>
  <br />
  <input type="radio" name="options" value="Bravery" />
  <label>Bravery</label>
  <br />
  <button class="send" type="button">Confirmar</button>

  <section class="result" v-if="this.answerSubmitted">
    <template v-if="this.chosen_answer == this.correctAnswer">
      <h4>
        &#9989; Parabéns, a resposta "{{ this.correctAnswer }}" está correta.
      </h4>
    </template>
    <template v-else>
      <h4>
        &#10060; Que pena, a resposta está errada. A resposta correta é "{{
          this.correctAnswer
        }}".
      </h4>
    </template>
    <button @click="this.getNewQuestion()" class="send" type="button">
      Próxima pergunta
    </button>
  </section>
</template>

<script>
export default {
  name: "App",
  created() {
    this.axios
      .get("https://opentdb.com/api.php?amount=10&category=18&type=multiple")
      .then((response) => {
        console.log(response.data);
      });
  },
};

// https://opentdb.com/api.php?amount=10&category=18&type=multiple
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
