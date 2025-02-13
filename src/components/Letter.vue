# Den Code weiter anpassen, um sicherzustellen, dass der "Nein"-Button korrekt
positioniert ist. final_adjusted_code = """
<template>
  <div
    class="absolute left-1/2 top-1/2 w-full max-w-[90%] sm:max-w-[700px] aspect-[1.4/1] rounded-lg p-6 flex items-center justify-center cursor-pointer transform -translate-x-1/2 -translate-y-1/2"
  >
    <div
      v-if="!clickedYes"
      class="w-[400px] h-[500px] bg-[#F9A8D4] rounded-lg shadow-lg flex flex-col items-center justify-center text-center p-8 pt-16 relative"
    >
      <h1 class="text-4xl font-bold text-[#E74C3C] mb-6">
        Will you be my Valentine?
      </h1>
      <div class="space-y-4 flex flex-col items-center flex-1">
        <button
          ref="yesButton"
          @click="handleYes"
          class="px-6 py-3 bg-[#E74C3C] text-white font-bold text-lg rounded-lg shadow-md hover:bg-[#c0392b] transition-transform duration-200 transform hover:scale-105 mb-4"
        >
          Yes!
        </button>
      </div>
      <button
        @mouseover="moveButton"
        :style="{
          top: `${buttonPosition.top}px`,
          left: `${buttonPosition.left}px`,
        }"
        class="absolute text-[#E74C3C] text-xs md:text-sm rounded-lg px-3 md:px-4 py-1.5 md:py-2 bg-white"
      >
        No
      </button>
    </div>
    <div v-if="clickedYes">
      <div
        class="w-[400px] cursor-auto h-[500px] bg-[#F9A8D4] rounded-lg shadow-lg flex flex-col items-center justify-center text-center p-0"
      >
        <div class="mb-6">
          <img
            src="../assets/kiss.gif"
            alt="Bears"
            class="w-[200px] h-auto mx-auto rounded-md shadow-md"
          />
        </div>
        <h1 class="text-4xl font-bold text-[#E74C3C] mb-4">Yay!</h1>
        <p class="text-lg text-gray-700 mb-6">
          🎉 Cooool! Reply me on Whatsapp!
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive, onMounted, ref, nextTick } from "vue";

const clickedYes = ref(false);
const buttonPosition = reactive({
  top: 0,
  left: 0,
});

const yesButton = ref(null);

const moveButton = () => {
  const randomTop = Math.floor(Math.random() * 350) + 50; // Zufällige Position
  const randomLeft = Math.floor(Math.random() * 300) + 50;
  buttonPosition.top = randomTop;
  buttonPosition.left = randomLeft;
};

const setInitialButtonPosition = () => {
  if (yesButton.value) {
    const btnRect = yesButton.value.getBoundingClientRect();
    const container = yesButton.value.closest(".relative"); // Container des Buttons

    if (container) {
      const containerRect = container.getBoundingClientRect();
      buttonPosition.top = btnRect.bottom - containerRect.top + 10; // Direkt unter dem "Ja!"-Button
      buttonPosition.left = btnRect.left - containerRect.left; // Gleiche horizontale Ausrichtung
    }
  }
};

onMounted(async () => {
  await nextTick(); // Sicherstellen, dass der DOM vollständig gerendert ist
  setInitialButtonPosition();
});

const handleYes = () => {
  clickedYes.value = true;
  fetch("https://foryou.crothmann.de/api/sendemail", {
    method: "POST",
    body: JSON.stringify({ response: "yes" }),
  }).then((res) => console.log(res));
};
</script>

<style scoped>
@keyframes fade-in {
  from {
    opacity: 0;
    transform: scale(0.5);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

.animate-fade-in {
  animation: fade-in 0.8s ease-out forwards;
}
</style>
