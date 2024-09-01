<script setup>
import { computed, ref } from "vue";

const questions = ref([
  {
    question: "Où se trouve le Gabon ?",
    answer: 4,
    options: [
      "En Europe",
      "En Afrique du Nord",
      "En Afrique du Sud ",
      "En Afrique de l'Ouest",
      "Au Centre Afrique"
    ],
    selected: null
  },
  {
    question: "Quel sont les couleurs du Gabon?",
    answer: 2,
    options: [
      "Bleu - Blanc - Rouge",
      "Jaune - Bleu",
      "Vert - Jaune - Bleu ",
      "Rouge - Blanc - Rouge",
      "Noir - Orange - Rouge"
    ],
    selected: null
  },
  {
    question: "Quelle est la capitale du Gabon ?",
    answer: 1,
    options: ["Estuaire", "Libreville", "Brazzaville ", "Franceville"],
    selected: null
  },
  {
    question: "Quelle est sa superficie ?",
    answer: 3,
    options: [
      "500 000 km²",
      "100 000 km²",
      "50 000 km² ",
      "247 667 km²",
      "344 505 km²"
    ],
    selected: null
  }
]);

const quizCompleted = ref(false);
const currentQuestion = ref(0);
const score = computed(() => {
  let value = 0;
  questions.value.map((q) => {
    if (q.selected === q.answer) {
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

const SetAnswer = (evt) => {
  questions.value[currentQuestion.value].selected = evt.target.value;
  evt.target.value = null;
};

const nextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++;
  } else {
    quizCompleted.value = true;
  }
};
</script>

<template>
  <main class="app">
    <h1>The Gowther's Quizz</h1>

    <section class="quiz" v-if="!quizCompleted">
      <div class="quiz-info">
        <span class="question">{{ getCurrentQuestion.question }}</span>
      </div>

      <div class="option">
        <label
          v-for="(option, index) in getCurrentQuestion.options"
          :key="index"
          :class="`option ${
            getCurrentQuestion.selected == index
              ? index == getCurrentQuestion.answer
                ? 'Vrai'
                : 'Faux'
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
            :name="getCurrentQuestion.index"
            :value="index"
            v-model="getCurrentQuestion.selected"
            :disabled="getCurrentQuestion.selected"
            @click="SetAnswer"
          />
          <span>{{ option }}</span>
        </label>
      </div>

      <button @click="nextQuestion" :disabled="!getCurrentQuestion.selected">
        {{
          getCurrentQuestion.index == questions.length - 1
            ? "Terminé"
            : getCurrentQuestion.selected == null
            ? "Choisissez une réponse"
            : "Suivant"
        }}
      </button>
    </section>

    <section v-else>
      <h2>Bravo vous avez fini mon quizz !</h2>
      <p>Vous avez reussi {{ score }}/{{ questions.length }}</p>
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
  min-height: 100hv;
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
  border-radius: 0.5rem;
}

.quiz-info {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}

.quiz-info .question {
  color: #fff;
  font-size: 1.25rem;
  text-align: center;
  margin: 0 auto;
  font-weight: bold;
}

.quiz-info .score {
  color: #fff;
  font-size: 1.25rem;
}

.options {
  margin-bottom: 1rem;
}

.option {
  padding: 1rem;
  display: block;
  margin-bottom: 0.5rem;
  cursor: pointer;
  border-radius: 0.5rem;
  background-color: #271c36;
}

.option:hover {
  background-color: #2d213f;
}

.option.Vrai {
  background-color: #2cce7d;
}

.option.Faux {
  background-color: #ff5e5e;
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
  border-radius: 0.5rem;
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1.25rem;
  margin-top: 0.5rem;
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
  font-size: 1.25rem;
  text-align: center;
}
</style>
