<template>
  <div v-if="answers && question">
    <h1 v-html="question"></h1>

    <template v-for="answer in answers" :key="answer">
      <input
        type="radio"
        name="options"
        v-model="chosenAnswer"
        :value="answer"
        :disabled="answerSubmitted"
      />
      <label v-html="answer"></label>
      <br />
    </template>

    <button
      v-if="!answerSubmitted"
      class="send"
      type="button"
      @click="sendAnswer()"
      :disabled="answerSubmitted"
    >Send</button>

    <section class="result" v-if="answerSubmitted">
      <h4
        v-if="chosenAnswer == correctAnswer"
      >✅ Congratulations, the answer "{{ correctAnswer }}" is correct.</h4>
      <h4 v-else>❌ I'm sorry, you picked the wrong answer. The correct is "{{ correctAnswer }}".</h4>

      <button @click="getNewQuestion()" class="send" type="button">Next question</button>
    </section>
  </div>
  <div v-else>
    <h1>Loading ...</h1>
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
      correctAnswer: "",
      chosenAnswer: undefined,
      answerSubmitted: false
    }
  },
  computed: {
    answers() {
      const answers: Array<string> = [...this.incorrectAnswers]
      answers.splice((Math.round(Math.random() * answers.length)), 0, this.correctAnswer)
      return answers
    }
  },
  methods: {
    sendAnswer: function () {
      if (!this.chosenAnswer) alert('Pick one of the options')
      else if (this.chosenAnswer === this.correctAnswer) console.log('you got it rigth')
      else if (this.chosenAnswer !== this.correctAnswer) console.log('you loose')
      this.answerSubmitted = true
    },
    getNewQuestion: function () {
      console.log("yo")
    }
  },
  async created() {
    const response = await fetch("https://opentdb.com/api.php?amount=1").then(res => res.json())
    this.question = response.results[0].question
    this.incorrectAnswers = response.results[0].incorrect_answers
    this.correctAnswer = response.results[0].correct_answer
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
