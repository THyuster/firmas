<template>
  <div>
    <canvas id="lienzo" width="400" height="200"></canvas>
    <button @click="guardarFirma">Guardar Firma</button>
    <button @click="borrarFirma">Borrar Firma</button>
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
    async guardarFirma() {
      var canvas = document.getElementById('lienzo');
      var dataURL = canvas.toDataURL(); // Convierte el contenido del lienzo a una URL de datos (base64)

      try {
        // Realiza una solicitud POST a la API con la imagen en formato base64
        const response = await fetch('URL_DE_TU_API', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({ image: dataURL }),
        });

        if (response.ok) {
          console.log('Imagen guardada exitosamente en la API.');
        } else {
          console.error('Error al guardar la imagen en la API.');
        }
      } catch (error) {
        console.error('Error de red:', error);
      }
    },
    borrarFirma() {
      var canvas = document.getElementById('lienzo');
      var contexto = canvas.getContext('2d');
      contexto.clearRect(0, 0, canvas.width, canvas.height); // Limpia el contenido del lienzo
    },
  },
};
</script>

<style scoped>
#lienzo {
  border: 1px solid #000; /* Establece el borde del lienzo */
}

button {
  margin-top: 10px;
}
</style>