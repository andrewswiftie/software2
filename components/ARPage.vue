<template>
    <div class="ar-page">
      <div class="background-blur"></div>
      <h1 class="ar-page-title">Prueba de Realidad Aumentada</h1>
      <div v-if="videoStream" class="video-container">
        <video :srcObject="videoStream" autoplay></video>
      </div>
      <div v-else>
        <p>Permiso denegado o cámara no disponible</p>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        videoStream: null,
      };
    },
    async mounted() {
      // Solicita permiso para acceder a la cámara
      try {
        const stream = await navigator.mediaDevices.getUserMedia({ video: true });
        // Si se concede el permiso, guarda el stream y úsalo en la plantilla
        this.videoStream = stream;
      } catch (error) {
        console.error('Error al acceder a la cámara:', error);
        // Maneja el error, por ejemplo, mostrar un mensaje al usuario
      }
    },
    beforeUnmount() {
      // Detén el stream cuando el componente se desmonte
      if (this.videoStream) {
        const tracks = this.videoStream.getTracks();
        tracks.forEach(track => track.stop());
      }
    }
  }
  </script>
  
  <style scoped>
  /* Estilos para la página de Realidad Aumentada */
  .ar-page {
    width: 100vw;
    height: 100vh;
    position: relative;
    overflow: hidden; /* Deshabilitar el desplazamiento */
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  
  /* Estilos para el fondo con desenfoque */
  .background-blur {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-image: url('C:\Users\Andre\Downloads\Proyecto\Proyecto\image2.png');
    background-size: cover;
    background-position: center;
    filter: blur(5px); /* Ajusta la intensidad del desenfoque */
    z-index: -1; /* Asegura que el fondo esté detrás de otros elementos */
  }
  
  /* Estilos para el título "Página de Realidad Aumentada" */
  .ar-page-title {
    font-size: 2.5rem; /* Tamaño de fuente ajustado */
    font-family: 'Roboto', sans-serif; /* Fuente elegante */
    color: #fff; /* Color de texto blanco */
    margin-bottom: 20px; /* Espaciado inferior */
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5); /* Sombra de texto suave */
    text-align: center; /* Alineación centrada */
  }
  
  /* Estilos para el contenedor del video */
  .video-container {
    flex: 1;
    width: 100%;
    height: 100%;
    overflow: hidden;
  }
  
  video {
    width: 100%;
    height: 100%;
    object-fit: contain; /* Ajusta el tamaño del video manteniendo las proporciones */
  }
  </style>