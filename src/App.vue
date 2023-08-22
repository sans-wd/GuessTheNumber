<script setup>
import { ref } from "vue";
const randomNum = ref(null);
const inputNumber = ref(null);
const score = ref(20);
const HighestScore = ref(0);
const numBox = ref("?");
const result = ref("Try to guess");
const disable = ref(false);
const win = ref(false);
const loose = ref(false);
const inpNum = () => {
  if (!disable.value) {
    if (inputNumber.value == null) {
      result.value = "Its not a number";
    }
    if (inputNumber.value < randomNum.value && inputNumber.value != null) {
      score.value -= 1;
      result.value = "Too low";
    }
    if (inputNumber.value > randomNum.value) {
      score.value--;
      result.value = "Too high";
    }
    if (inputNumber.value == randomNum.value) {
      disable.value = true;
      win.value = true;
      if (score.value > HighestScore.value) {
        HighestScore.value = score.value;
      }
      numBox.value = randomNum.value;
      result.value = "You win";
    }
    if (score.value == 0) {
      disable.value = true;
      loose.value = true;
      numBox.value = randomNum.value;
      result.value = "You lose";
    }
  }
};
const again = () => {
  inputNumber.value = null;
  randomNum.value = Math.floor(Math.random() * 19) + 1;
  score.value = 20;
  disable.value = false;
  win.value = false;
  loose.value = false;
  numBox.value = "?";
  result.value = "Try to guess";
  console.log("game start", randomNum.value);
};
again();
</script>

<template>
  <main
    :class="{ ' bg-green-600': win, 'bg-red-600': loose }"
    class="bg-black h-screen flex justify-center items-center"
  >
    <div class="container mx-auto">
      <div class="header flex justify-between items-center">
        <div class="logo">
          <font-awesome-icon
            class="text-6xl text-white"
            :icon="['fas', 'arrow-up-9-1']"
          />
        </div>
        <button
          @click="again"
          class="bg-white hover:bg-gray-100 text-gray-800 font-semibold py-2 px-4 border border-gray-400 rounded shadow"
        >
          Again
        </button>
      </div>
      <div class="number flex flex-col justify-center items-center my-24">
        <h1 class="text-5xl text-white text-center mb-8">Guess My Number!</h1>
        <div
          class="box text-5xl h-36 w-36 bg-white border-black border-2 flex justify-center items-center rounded-lg"
        >
          {{ numBox }}
        </div>
      </div>

      <div class="userInOut flex justify-between items-center">
        <div class="input flex flex-col gap-1">
          <input
            v-model.number="inputNumber"
            class="bg-white border-2 border-black w-32 h-11 rounded outline-none p-2 text-center"
            type="number"
          />

          <button
            @click="inpNum()"
            type="submit"
            :class="{ disbaled: disable }"
            class="bg-white hover:bg-gray-100 text-gray-800 font-semibold py-2 px-4 w-32 h-11 border-2 border-black rounded shadow"
          >
            Check
          </button>
        </div>
        <div class="output flex flex-col gap-1 text-white font-bold text-2xl">
          <h1>{{ result }}</h1>
          <h1>Score: {{ score }}</h1>
          <h3>Highest score: {{ HighestScore }}</h3>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
.disbaled {
  cursor: not-allowed;
  opacity: 75%;
}
</style>
