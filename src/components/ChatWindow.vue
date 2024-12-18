<template>
    <div class="h-screen flex items-center justify-center bg-gradient-to-br from-indigo-500 to-blue-700 p-4">
      <!-- Contenedor del chat -->
      <div class="w-full max-w-4xl bg-white shadow-2xl rounded-lg flex flex-col overflow-hidden">
        <!-- Header del chat -->
        <div class="bg-gradient-to-r from-blue-500 to-indigo-500 text-white text-center py-4">
          <h2 class="text-3xl font-semibold">
            Chat con {{ selectedAgent.name }}
          </h2>
        </div>
  
        <!-- Área de mensajes -->
        <div class="flex-1 p-4 overflow-y-auto space-y-2 bg-gray-50">
          <div
            v-for="(message, index) in messages"
            :key="index"
            class="flex"
            :class="{
              'justify-start': message.sender !== 'Usuario',
              'justify-end': message.sender === 'Usuario',
            }"
          >
            <div
              class="max-w-sm px-4 py-2 rounded-lg shadow"
              :class="{
                'bg-indigo-500 text-white': message.sender === 'Usuario',
                'bg-gray-200 text-gray-700': message.sender !== 'Usuario',
              }"
            >
              <strong v-if="message.sender !== 'Usuario'">{{ message.sender }}:</strong>
              {{ message.text }}
            </div>
          </div>
        </div>
  
        <!-- Input y botón -->
        <div class="flex p-4 bg-gray-100">
          <input
            v-model="newMessage"
            type="text"
            placeholder="Escribe tu mensaje..."
            class="flex-grow p-2 rounded-l-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-indigo-500"
            @keyup.enter="sendMessage"
          />
          <button
            @click="sendMessage"
            class="bg-indigo-500 text-white px-6 py-2 rounded-r-lg hover:bg-indigo-600 transition"
            :disabled="loading"
          >
            Enviar
          </button>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from "axios";
  
  export default {
    props: ["selectedAgent"],
    data() {
      return {
        messages: [],
        newMessage: "",
        loading: false,
      };
    },
    methods: {
      async sendMessage() {
        if (!this.newMessage.trim()) return;
  
        // Añadir mensaje del usuario
        this.messages.push({ sender: "Usuario", text: this.newMessage });
        this.loading = true;
  
        try {
          // Llamada a la API
          const payload = {
            agentId: this.selectedAgent.id, 
            messages: [{ role: "user", content: this.newMessage }],
            stream: false,
            format: "json",
          };
  
          const response = await axios.post(
            "https://api.codegpt.co/api/v1/chat/completions",
            payload,
            {
              headers: {
                Authorization: `Bearer ${process.env.VUE_APP_CODEGPT_API_KEY}`,
                "CodeGPT-Org-Id": process.env.VUE_APP_CODEGPT_ORG_ID,
                "Content-Type": "application/json",
              },
            }
          );
  
          
          const assistantResponse =
            response.data.choices[0]?.message?.completion || "Sin respuesta del agente.";
  
          this.messages.push({
            sender: this.selectedAgent.name,
            text: assistantResponse,
          });
        } catch (error) {
          console.error("Error al enviar mensaje:", error);
          this.messages.push({
            sender: "Sistema",
            text: "Error al enviar el mensaje.",
          });
        } finally {
          this.loading = false;
          this.newMessage = "";
        }
      },
    },
  };
  </script>
  
  <style scoped>
 
  ::-webkit-scrollbar {
    width: 6px;
  }
  ::-webkit-scrollbar-thumb {
    background-color: rgba(0, 0, 0, 0.2);
    border-radius: 3px;
  }
  </style>
  