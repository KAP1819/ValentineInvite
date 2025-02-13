# Increase the duration of the slide animation to 1.5 seconds (1500ms).
updated_code = """
<template>
  <div
    class="min-h-screen bg-[#FDF6EC] flex items-center justify-center p-4 overflow-hidden w-full"
  >
    <Letter v-if="showLetter" />
    <div class="relative w-full max-w-2xl mx-auto mt-24">
      <div
        @click="handleClick"
        :class="[
          isClicked
            ? 'scale-110 transition-transform duration-200 ease-out'
            : 'hover:scale-105 transition-transform duration-300 ease-in-out',
          isSliding
            ? 'translate-y-[100vh] transition-transform duration-[1500ms] ease-in-out'
            : '',
        ]"
        @transitionend="startSlide"
        class="absolute left-1/2 top-1/2 w-full max-w-[90%] sm:max-w-[700px] aspect-[1.4/1] bg-[#E74C3C] rounded-lg shadow-lg p-6 flex items-center justify-center cursor-pointer transform -translate-x-1/2 -translate-y-1/2"
      >
        <div class="absolute top-4 right-4 md:size-24 size-16">
          <img
            src="./assets/stamp.png"
            alt="Decorative stamp"
            class="w-full h-full object-contain"
          />
        </div>
        <div class="absolute inset-0 flex items-center justify-center">
          <p
            class="text-white text-3xl md:text-5xl"
            style="font-family: Caveat Variable; font-weight: 700"
          >
            For Kavita ;)
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import Letter from "./components/Letter.vue";
const isClicked = ref(false);
const isSliding = ref(false);
const showLetter = ref(false);
const handleClick = () => {
  isClicked.value = true;
};

const startSlide = () => {
  if (isClicked.value) {
    showLetter.value = true;
    setTimeout(() => {
      isSliding.value = true;
    }, 700); // The letter appears a little later after the slide effect starts
  }
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
