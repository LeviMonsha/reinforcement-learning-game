<template>
  <div class="flex items-center justify-center h-screen bg-gray-900">
    <div
      class="relative w-full h-full max-w-4xl max-h-3xl bg-gray-800 rounded-lg shadow-lg overflow-hidden"
    >
      <div
        class="bg-black rounded-lg border-4 border-gray-600 flex items-center justify-center h-[80%]"
      >
        <button
          v-if="selectedMode"
          @click="startGame"
          class="mt-8 px-8 py-4 bg-blue-500 text-white text-lg rounded hover:bg-blue-600 focus:outline-none"
        >
          СТАРТ
        </button>
      </div>
      <div
        class="absolute bottom-0 left-0 right-0 p-4 flex flex-col md:flex-row justify-between bg-gray-700"
      >
        <button
          @click="toggleMode('manual')"
          :class="[
            'px-4 py-2 rounded mb-2 md:mb-0',
            selectedMode === 'manual'
              ? 'bg-blue-500 text-white'
              : 'bg-gray-700 text-gray-300',
          ]"
        >
          Ручное управление
        </button>
        <button
          @click="toggleMode('q-learning')"
          :class="[
            'px-4 py-2 rounded',
            selectedMode === 'q-learning'
              ? 'bg-green-500 text-white'
              : 'bg-gray-700 text-gray-300',
          ]"
        >
          Q-learning
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const selectedMode = ref("");

const toggleMode = (mode) => {
  if (selectedMode.value === mode) {
    selectedMode.value = "";
  } else {
    selectedMode.value = mode;
  }
};

// eslint-disable-next-line no-undef
const emit = defineEmits(["start"]);

const startGame = () => {
  emit("start", selectedMode.value);
};
</script>
