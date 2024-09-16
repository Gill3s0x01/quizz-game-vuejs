<!-- eslint-disable vuejs-accessibility/label-has-for -->
<template>
  <div>
    <template v-if="this.question">
      <h1 v-html="this.question"></h1>
      <template v-for="(answer, index) in this.answers" v-bind:key="index" >
        <input
        :disabled="this.answerSubmitted"
          type="radio"
          name="options"
          :value="answer"
          v-model="this.chosenAnswer"
        >
        <label v-html="answer"></label><br>
      </template>
      <button
        v-if="!this.answerSubmitted"
        class="send"
        type="button"
        @click="this.sendAnswer()"
        :disabled="this.answerSubmitted">Send</button>
    </template>
  </div>
  <section v-if="this.answerSubmitted">
    <h4 v-if="this.chosenAnswer == this.correctAnswer">
      &#9989; Congratulations,
       the answer
        "{{this.correctAnswer}}" is correct
    </h4>
    <h4 v-else>
      &#10060;
        I'm sorry,
        you picked the wrong answer. The correct is
       "{{this.correctAnswer}}"
    </h4>
    <button class="send" type="button">Next question</button>
  </section>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue';

export default {
  name: 'App',
  data() {
    return {
      question: undefined,
      correctAnswer: undefined,
      incorrectAnswer: undefined,
      chosenAnswer: undefined,
      answerSubmitted: false,
    };
  },
  computed: {
    answers() {
      const answers = [...this.incorrectAnswer];
      answers.splice(Math.round(Math.random()), 0, this.correctAnswer);
      return answers;
    },
  },
  methods: {
    sendAnswer() {
      if (!this.chosenAnswer) {
        console.log('Please choose an answer');
      } else {
        this.answerSubmitted = true;
        if (this.chosenAnswer === this.correctAnswer) {
          console.log('You got it right');
        } else {
          console.log('You got it wrong');
        }
      }
    },
  },
  created() {
    this.axios.get('https://opentdb.com/api.php?amount=1&category=15&difficulty=medium')
      .then((response) => {
        this.question = response.data.results[0].question;
        this.correctAnswer = response.data.results[0].correct_answer;
        this.incorrectAnswer = response.data.results[0].incorrect_answers;
      });
  },

  components: {
    // HelloWorld,
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
  margin-top: 60px auto;
  max-width: 960px;
  input[type=radio] {
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
    border-radius: 5px;
    cursor: pointer;

    &:hover {
      background-color: #b9c9d8;
      border-color: #b9c9d8;
      color: #1867c0;
      transition: ease-in-out 0.3s;
    }
  }
}
</style>
