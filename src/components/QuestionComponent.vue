<template>
  <div>
    <h2>{{ question.text }}</h2>
    <div>
      <ul>
        <li v-for="(option, index) in question.options" :key="index">
          <label>
            <input type="radio" :value="option.charAt(0)" v-model="selectedAnswer">
            {{ option }}
          </label>
        </li>
      </ul>
    </div>
    <button @click="submitAnswer">Submit Answer</button>
  </div>
</template>

<script>
export default {
  props: ['question'],
  data() {
    return {
      selectedAnswer: null,
    };
  },
  methods: {
    submitAnswer() {
      if (this.selectedAnswer !== null) {
        this.$emit('answer', this.selectedAnswer);
        this.selectedAnswer = null;
      } else {
        alert('Please select an answer.');
      }
    },
  },
};
</script>

<style scoped>
/* Scoped CSS for QuestionComponent */
ul {
  list-style-type: none;
  padding: 0;
}
li {
  margin-bottom: 10px;
}
button {
  margin-top: 10px;
  padding: 10px 20px;
  background-color: #007BFF;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 5px;
}
button:hover {
  background-color: #0056b3;
}
</style>
