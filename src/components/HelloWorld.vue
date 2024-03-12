<template>
  <div>
    <canvas id="lienzo" width="400" height="200"></canvas>
    <button @click="guardarFirma">Guardar Firma</button>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  mounted() {
    this.initializeCanvas();
  },
  methods: {
    initializeCanvas() {
      var canvas = document.getElementById('lienzo');
      var contexto = canvas.getContext('2d');
      var dibujando = false;

      canvas.addEventListener('mousedown', function (e) {
        dibujando = true;
        contexto.beginPath();
        contexto.moveTo(e.clientX - canvas.offsetLeft, e.clientY - canvas.offsetTop);
      });

      canvas.addEventListener('mousemove', function (e) {
        if (dibujando) {
          contexto.lineTo(e.clientX - canvas.offsetLeft, e.clientY - canvas.offsetTop);
          contexto.stroke();
        }
      });

      canvas.addEventListener('mouseup', function () {
        dibujando = false;
        contexto.closePath();
      });

      canvas.addEventListener('mouseleave', function () {
        dibujando = false;
        contexto.closePath();
      });
    },
    guardarFirma() {
      var canvas = document.getElementById('lienzo');
      var dataURL = canvas.toDataURL(); // Convierte el contenido del lienzo a una URL de datos (base64)

      // Crea un enlace temporal y simula un clic para descargar la imagen
      var enlace = document.createElement('a');
      enlace.href = dataURL;
      enlace.download = 'firma.png';
      enlace.click();
    }
  }
}
</script>

<style scoped>
#lienzo {
  border: 1px solid #000; /* Establece el borde del lienzo */
}

button {
  margin-top: 10px;
}
</style>