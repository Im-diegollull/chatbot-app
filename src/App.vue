<template>
  <div id="app" class="h-screen bg-gray-100 flex justify-center items-center">
    <!-- Página principal -->
    <MyHome v-if="!chatStarted" @startChat="startChat" class="max-w-4xl" />

    <!-- Chat principal -->
    <div
      v-else
      class="flex flex-col items-center justify-center h-screen bg-gradient-to-br from-indigo-500 to-purple-700 p-4"
    >
      <div class="w-full max-w-4xl bg-white rounded-lg shadow-2xl p-6">
        <AgentSelector @selectAgent="setAgent" v-if="!selectedAgent" />
        <ChatWindow v-else :selectedAgent="selectedAgent" />
      </div>
    </div>
  </div>
</template>

<script>
import MyHome from "./components/MyHome.vue";
import AgentSelector from "./components/AgentSelector.vue";
import ChatWindow from "./components/ChatWindow.vue";

export default {
  components: { MyHome, AgentSelector, ChatWindow },
  data() {
    return {
      chatStarted: false,
      selectedAgent: null,
    };
  },
  methods: {
    startChat() {
      this.chatStarted = true;
    },
    setAgent(agent) {
      this.selectedAgent = agent;
    },
  },
};
</script>

<style>
body {
  margin: 0;
  font-family: "Arial", sans-serif;
}
</style>
