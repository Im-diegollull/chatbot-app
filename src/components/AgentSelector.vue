<template>
    <div class="flex flex-col items-center justify-center space-y-6 p-8 bg-white rounded-lg shadow-lg">
      <h2 class="text-3xl font-bold text-indigo-600">Seleccionar Agente</h2>
      <select
        v-model="selected"
        @change="emitAgent"
        class="w-2/3 p-2 border-2 border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500 transition"
      >
        <option disabled value="">Selecciona un agente</option>
        <option
          v-for="agent in agents"
          :key="agent.id"
          :value="agent"
          class="text-gray-700"
        >
          {{ agent.name }}
        </option>
      </select>
      <p v-if="loading" class="text-gray-500">Cargando agentes...</p>
      <p v-if="error" class="text-red-500 font-bold text-lg">{{ error }}</p>
    </div>
  </template>

  <script>
  import axios from "axios";

  export default {
    data() {
      return {
        agents: [],
        selected: "",
        loading: false,
        error: null,
      };
    },
    async created() {
      await this.fetchAgents();
    },
    methods: {
      async fetchAgents() {
        this.loading = true;
        this.error = null;
        try {
          const response = await axios.get("https://api.codegpt.co/api/v1/agent", {
            headers: {
              Authorization: `Bearer ${process.env.VUE_APP_CODEGPT_API_KEY}`,
              "CodeGPT-Org-Id": process.env.VUE_APP_CODEGPT_ORG_ID,
            },
          });
          this.agents = response.data;
        } catch (err) {
          this.error = "Error al cargar los agentes.";
        } finally {
          this.loading = false;
        }
      },
      emitAgent() {
        this.$emit("selectAgent", this.selected);
      },
    },
  };
  </script>

  <style scoped>
  select {
    transition: all 0.3s ease;
  }
  select:focus {
    border-color: #4f46e5;
    box-shadow: 0 0 0 3px rgba(79, 70, 229, 0.2);
  }
  .text-lg {
    font-size: 1.125rem;
    line-height: 1.75rem;
  }
  </style>