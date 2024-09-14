<template>
  <div class="app">
    <h1>Chat entre desconocidos</h1>
    <!-- Mostrar contenido solo si hay exactamente 2 usuarios -->
    <div v-if="users.length === 2">
      <!-- Mostrar nombres completos de los usuarios -->
      <h3>Conversación entre {{ users[0].name.first }} {{ users[0].name.last }} y {{ users[1].name.first }} {{ users[1].name.last }}</h3>

      <div class="chat-container">
        <!-- Entrada y botón de envío de mensajes del Usuario 1 -->
        <div class="user-chat">
          <h4>{{ users[0].name.first }} {{ users[0].name.last }}</h4>
          <!-- Mostrar la foto del Usuario 1 -->
          <img :src="users[0].picture.thumbnail" :alt="'Foto de ' + users[0].name.first" />
          <!-- Campo de texto para el mensaje del Usuario 1 -->
          <textarea v-model="newMessageUser1" class="form-control" rows="5" placeholder="Escribe un mensaje..."></textarea>
          <!-- Botón para enviar el mensaje del Usuario 1 -->
          <button @click="sendMessage(0)" class="send-button">Enviar Mensaje</button>
        </div>

        <!-- Mostrar mensajes en el centro -->
        <div class="conversation">
          <ChatComponent :messages="messages" :users="users" />
        </div>

        <!-- Entrada y botón de envío de mensajes del Usuario 2 -->
        <div class="user-chat">
          <h4>{{ users[1].name.first }} {{ users[1].name.last }}</h4>
          <!-- Mostrar la foto del Usuario 2 -->
          <img :src="users[1].picture.thumbnail" :alt="'Foto de ' + users[1].name.first" />
          <!-- Campo de texto para el mensaje del Usuario 2 -->
          <textarea v-model="newMessageUser2" class="form-control" rows="5" placeholder="Escribe un mensaje..."></textarea>
          <!-- Botón para enviar el mensaje del Usuario 2 -->
          <button @click="sendMessage(1)" class="send-button">Enviar Mensaje</button>
        </div>
      </div>
    </div>
    <!-- Mensaje de carga si no se han obtenido 2 usuarios aún -->
    <div v-else>
      <p>Cargando usuarios...</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import ChatComponent from './components/ChatComponent.vue'; // Asegúrate de que el componente esté en la carpeta correcta

export default {
  data() {
    return {
      users: [],  // Array para almacenar los usuarios obtenidos de la API
      newMessageUser1: '',  // Mensaje nuevo del Usuario 1
      newMessageUser2: '',  // Mensaje nuevo del Usuario 2
      messages: [],  // Array para almacenar los mensajes del chat
      userColors: ['#a8d5e2', '#f7c8c9']  // Colores específicos para cada usuario
    };
  },
  methods: {
    async fetchUsers() {
      try {
        // Realiza una solicitud para obtener 2 usuarios aleatorios
        const response = await axios.get('https://randomuser.me/api/?results=2');
        this.users = response.data.results;  // Guarda los usuarios obtenidos
      } catch (error) {
        console.error('Error al obtener usuarios:', error);
      }
    },
    sendMessage(userIndex) {
      // Determina cuál usuario está enviando el mensaje
      const message = userIndex === 0 ? this.newMessageUser1 : this.newMessageUser2;
      if (message.trim()) {
        const owner = this.users[userIndex];  // Obtiene el usuario que envía el mensaje
        this.messages.push({
          text: message,
          color: this.userColors[userIndex],  // Aplica el color correspondiente al usuario
          owner: `${owner.name.first} ${owner.name.last}`  // Incluye el nombre completo del propietario
        });
        // Limpia el campo de texto después de enviar el mensaje
        if (userIndex === 0) {
          this.newMessageUser1 = '';
        } else {
          this.newMessageUser2 = '';
        }
      }
    }
  },
  created() {
    this.fetchUsers();  // Llama a la función para obtener usuarios cuando se carga el componente
  },
  components: {
    ChatComponent
  }
};
</script>

<style scoped>
.app {
  text-align: center;
  margin-top: 20px;
}

.chat-container {
  border-radius: 10px 10px 0 0;
  padding: 20px;
  background-color: #f4f4f4;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 20px;
  margin-top: 20px;
}

.user-chat {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 30%;
}

.conversation {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 30%;
  background-color: #f4f4f4;
  padding: 10px;
  border-radius: 10px;
  overflow-y: auto;
  max-height: 400px;
}

img {
  border-radius: 50%;
  width: 70px;
  height: 70px;
  margin-bottom: 10px;
}

.form-control {
  margin-bottom: 10px;
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-size: 16px;
  resize: none;
}

.send-button {
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}

.send-button:hover {
  background-color: #0056b3;
}

.send-button:focus {
  outline: none;
}

.form-control:focus {
  border-color: #007bff;
  outline: none;
}
</style>
