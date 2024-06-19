<template>
  <div id="app">
    <div v-if="!started">
      <h1>Welcome to My Vue.js Quiz App!</h1>
      <p>Here you can take a quiz powered by OpenAI's GPT-3 to answer multiple-choice questions.</p>
      <button @click="startQuiz">Start Quiz</button>
    </div>
    <div v-else>
      <QuestionComponent v-if="question" :question="question" @answer="handleAnswer" />
      <FeedbackComponent v-if="showFeedback" :feedback="feedback" />
    </div>
  </div>
</template>

<script>
import axios from 'axios'; // Import axios here
import QuestionComponent from './components/QuestionComponent';
import FeedbackComponent from './components/FeedbackComponent';

export default {
  name: 'App',
  components: {
    QuestionComponent,
    FeedbackComponent,
  },
  data() {
    return {
      questions: [],
      currentQuestionIndex: 0,
      started: false,
      showFeedback: false,
      feedback: '',
      apiKey: 'sk-proj-Zv0MnRWWefMhgO8sFgBvT3BlbkFJeqNoG4kQO7etaOWFH03M',
    };
  },
  computed: {
    question() {
      return this.questions[this.currentQuestionIndex];
    },
  },
  methods: {
    startQuiz() {
      this.started = true;
      this.fetchQuestion();
    },
    fetchQuestion() {
      axios.post('https://api.openai.com/v1/engines/davinci-codex/completions', {
        prompt: 'Generate a multiple-choice question.',
        max_tokens: 50,
      }, {
        headers: {
          'Authorization': `Bearer ${this.apiKey}`,
          'Content-Type': 'application/json',
        },
      })
      .then(response => {
        console.log('API Response:', response.data);
        const questionText = response.data.choices[0].text.trim(); // Extract the generated question
        const options = ['a)', 'b)', 'c)', 'd)']; // Predefined options for simplicity
        const question = {
          text: questionText,
          options: options,
          answer: 'a', // Assuming the correct answer is always the first option for simplicity
        };
        this.questions.push(question);
        console.log('Questions:', this.questions); // Verify questions array
      })
      .catch(error => {
        console.error('Error fetching question:', error);
      });
    },
    handleAnswer(selectedAnswer) {
      // Check if answer is correct (for demonstration purposes)
      if (selectedAnswer === this.question.answer) {
        console.log('Correct!');
      } else {
        console.log('Incorrect!');
      }

      // Move to the next question or show feedback if all questions are answered
      if (this.currentQuestionIndex < this.questions.length - 1) {
        this.currentQuestionIndex++;
      } else {
        this.showFeedback = true;
        this.fetchFeedback(); // Fetch feedback when all questions are answered
      }
    },
    fetchFeedback() {
      // Simulating fetching feedback from an API (can be replaced with actual API call)
      // In this simplified example, feedback is hardcoded
      this.feedback = "Good job! You answered all questions.";
    },
  },
};
</script>

<style>
#app {
  font-family: Arial, sans-serif;
  text-align: center;
  margin-top: 20px;
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
