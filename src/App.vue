<template>
  <div>
    <h1 v-html="question"></h1>

    <input type="radio" name="options" value="true" />
    <label>True</label>
    <br />

    <input type="radio" name="options" value="false" />
    <label>False</label>
    <br />

    <button class="send" type="button">Send</button>
    <br />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'App',
  data() {
    return {
      question: "",
      incorrectAnswers: [],
      correctAnswers: "",
    }
  },
  computed: {
    answers() {
      const answers: Array<string> = [...this.incorrectAnswers]
      answers.push(this.correctAnswers)
      return answers
    }
  },
  async created() {
    const response = await fetch("https://opentdb.com/api.php?amount=1").then(res => res.json())
    this.question = response.results[0].question
    this.incorrectAnswers = response.results[0].incorrect_answers
    this.correctAnswers = response.results[0].correct_answer
  }
});

</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;

  input[type="radio"] {
    margin: 12px 4px;
  }

  button.send {
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    padding: 0 16px;
    color: #fff;
    background-color: #1867c0;
    border: 1px solid #1867c0;
    cursor: pointer;
  }
}
</style>
