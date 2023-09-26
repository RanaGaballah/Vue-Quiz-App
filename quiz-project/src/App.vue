<script setup>
import { ref, computed } from "vue";


const questions = ref([
  {
    question: "What is Vue?",
    answer: 0,
    options: ["A framework", "A library", "A programming language"],
    selected: null,
  },
  {
    question: "Why is Vue.js called a progressive framework?",
    answer: 0,
    options: ["Vue.js is called a progressive framework because it is being changed and developed continually.",
     "Vue.js is called a progressive framework because it facilitates us to create Dynamic User Interfaces and single-page applications.",
      "Vue.js is called a progressive framework because it follows the latest JavaScript standards.",
      "All of the above."
    ],
    selected: null,
  },
  {
    question: "Which of the following data binding interpolation is also known as 'Mustache' syntax?",
    answer: 2,
    options: ["v-on", "v-model", "{{}}", "[]"],
    selected: null,
  },
  {
    question: " Which of the following is the correct syntax to use for loop in Vue.js?",
    answer: 2,
    options: ["vFor", "v-For", "v-for","None of the above"],
    selected: null,
  },
  {
    question: "Which of the following syntax is correct for creating a Vue.js instance?",
    answer: 3,
    options: ["var text = new object ({//options})", "var text = new class ({//options})", "var text = new text ({//options})","var text = new Vue({// options })"],
    selected: null,
  },
  {
    question: "What is Vuex used for?",
    answer: 2,
    options: ["A framework", "Viewing things", "State management"],
    selected: null,
  },
  {
    question: "Which of the following is the advantage of using Vue.js?",
    answer: 3,
    options: [
      "Vue.js is very small in size.",
      "The documentation of Vue.js is very easy and comprehensive.",
      "Vue.js is flexible in nature.",
      "All of the above."
    ],
    selected: null,
  },
  {
    question: "Which of the following directive is used for two-way binding in Vue.js?",
    answer: 1,
    options: [
      "v-on",
      "v-model",
      "no-one",
      "v-bind"
    ],
    selected: null,
  },
  {
    question: "Which of the following directive is used for one-way data binding in Vue.js?",
    answer: 3,
    options: [
      "v-on",
      "v-model",
      "no-one",
      "v-bind"
    ],
    selected: null,
  },
  {
    question: "Which of the following statement is correct for components props in Vue.js?",
    answer: 3,
    options: [
      "Props are used to pass down data to the child components.",
      "Props are custom attributes that you can register on a component.",
      "When a value is passed to a prop attribute, it becomes a property on that component instance.",
      "All of the above."
    ],
    selected: null,
  },
]);

const quizCompleted = ref(false);
const currentQuestion = ref(0);
const score = computed(() => {
  let value = 0;
  questions.value.map((q) => {
    if (q.selected != null && q.answer == q.selected) {
      console.log("correct");
      value++;
    }
  });
  return value;
});

const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value];
  question.index = currentQuestion.value;
  return question;
});

const SetAnswer = (e) => {
  questions.value[currentQuestion.value].selected = e.target.value;
  e.target.value = null;
};

const NextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++;
    return;
  }

  quizCompleted.value = true;
};
</script>

<template>
  <main class="app">
    <h1>Vue Js 3 Quiz</h1>

    <section class="quiz" v-if="!quizCompleted">
      <div class="quiz-info">
        <span class="question">{{ getCurrentQuestion.question }}</span>
        <span class="score">Score {{ score }}/{{ questions.length }}</span>
      </div>

      <div class="options">
        <label
          v-for="(option, index) in getCurrentQuestion.options"
          :key="index"
          :for="'option' + index"
          :class="`option ${
            getCurrentQuestion.selected == index
              ? index == getCurrentQuestion.answer
                ? 'correct'
                : 'wrong'
              : ''
          } ${
            getCurrentQuestion.selected != null &&
            index != getCurrentQuestion.selected
              ? 'disabled'
              : ''
          }`"
        >
          <input
            type="radio"
            :id="'option' + index"
            :name="getCurrentQuestion.index"
            :value="index"
            v-model="getCurrentQuestion.selected"
            :disabled="getCurrentQuestion.selected"
            @change="SetAnswer"
          />
          <span>{{ option }}</span>
        </label>
      </div>

      <button @click="NextQuestion" :disabled="!getCurrentQuestion.selected">
        {{
          getCurrentQuestion.index == questions.length - 1
            ? "Finish"
            : getCurrentQuestion.selected == null
            ? "Select an option"
            : "Next question"
        }}
      </button>
    </section>

    <section v-else>
      <h2>You have finished the quiz!</h2>
      <p>Your score is {{ score }}/{{ questions.length }}</p>
    </section>
  </main>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Montserrat", sans-serif;
}

body {
  background-color: #271c36;
  color: #fff;
}

.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  height: 100vh;
}

h1 {
  font-size: 2rem;
  margin-bottom: 2rem;
}

.quiz {
  background-color: #382a4b;
  padding: 1rem;
  width: 100%;
  max-width: 640px;
}

.quiz-info {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}

.quiz-info .question {
  color: #8f8f8f;
  font-size: 1.25rem;
}

.quiz-info.score {
  color: #fff;
  font-size: 1.25rem;
}

.options {
  margin-bottom: 1rem;
}

.option {
  padding: 1rem;
  display: block;
  background-color: #271c36;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
}

.option:hover {
  background-color: #2d213f;
}

.option.correct {
  background-color: #2cce7d;
}

.option.wrong {
  background-color: #ff5a5f;
}

.option:last-of-type {
  margin-bottom: 0;
}

.option.disabled {
  opacity: 0.5;
}

.option input {
  display: none;
}

button {
  appearance: none;
  outline: none;
  border: none;
  cursor: pointer;
  padding: 0.5rem 1rem;
  background-color: #2cce7d;
  color: #2d213f;
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1.2rem;
  border-radius: 0.5rem;
}

button:disabled {
  opacity: 0.5;
}

h2 {
  font-size: 2rem;
  margin-bottom: 2rem;
  text-align: center;
}

p {
  color: #8f8f8f;
  font-size: 1.5rem;
  text-align: center;
}
</style>
