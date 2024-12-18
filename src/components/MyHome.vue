<template>
  <div class="h-screen flex flex-col items-center justify-center bg-gradient-to-br from-blue-500 to-indigo-700 text-white">
    <h1 class="text-6xl font-extrabold mb-6 animate-fadeIn">🚀 ¡Bienvenido a CodeGPT Chat!</h1>
    <p class="text-lg text-center mb-8 max-w-xl">
      Selecciona un agente inteligente y comienza a chatear de manera rápida y eficiente.
    </p>
    <button
      @click="$emit('startChat')"
      class="bg-indigo-600 text-white font-semibold px-6 py-3 rounded-lg shadow-lg hover:bg-indigo-700 transition duration-300 transform hover:scale-105"
    >
      Comenzar Chat
    </button>
  </div>
</template>

<script>
export default {
  name: "MyHome",
};
</script>

<style scoped>
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
.animate-fadeIn {
  animation: fadeIn 1.5s ease-out;
}
</style>
