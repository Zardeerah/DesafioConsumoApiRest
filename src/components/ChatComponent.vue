<template>
  <!-- Contenedor principal del componente del chat -->
  <div class="chat-component">
    <!-- Iteración sobre cada mensaje en el array 'messages' -->
    <div 
      v-for="(message, index) in messages" 
      :key="index"  
      class="message"  
      :style="{ backgroundColor: message.color }"  
    >
     
      <div>{{ message.owner  }}:</div>
      <!-- Mostrar el texto del mensaje -->
      <em>{{ message.text }}</em>
    </div>
  </div>
</template>

<script>
export default {
  // Definición de las propiedades que el componente acepta desde su componente padre
  props: {
    messages: Array  // La propiedad 'messages' es un array que contiene los mensajes del chat
  }
};
</script>

<style scoped>
/* Estilo para cada mensaje del chat */
.message {
  padding: 10px;  /* Espacio interno (padding) dentro del mensaje */
  margin: 5px 0;  /* Margen superior e inferior para separar mensajes */
  border-radius: 5px;  /* Bordes redondeados */
  color: rgb(22, 17, 17);  /* Color del texto en blanco */
  width: fit-content;  /* Ajusta el ancho del mensaje según el contenido */
  max-width: 100%;  /* Establece un ancho máximo del 60% para evitar que el mensaje sea demasiado ancho */
  text-align: center;  
}
</style>


  