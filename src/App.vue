<template>
  <div id="app">
    <h1>Actualizar imagenes Endo</h1>
    <p>
      <button @click="init">Actualizar</button>
    </p>
    <h2>Progreso</h2>
    <p>{{ imagenesSubidas }} de {{ totalImagenes }}</p>
    <input type="file" id="filechooser" />
    <p>
      <img ref="pruebaImagen" :src="imagen" alt="" />
    </p>
    <h3>{{ imagen }}</h3>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  data: () => ({
    imagenesSubidas: 0,
    totalImagenes: 0,
    imagenes: null,
    imagen: "",
  }),
  methods: {
    async init() {
      await this.getProducts();
      await this.procesarImagen();
    },
    async convertirBase64() {
      const xhr = new XMLHttpRequest();
      xhr.onload = function () {
        var reader = new FileReader();
        reader.onloadend = function () {
          return reader.result;
        };
        reader.readAsDataURL(xhr.response);
      };
      xhr.open("GET", this.imagen);
      xhr.responseType = "blob";
      xhr.send();
    },
    async procesarImagen() {
      this.totalImagenes = this.imagenes.length;
      // this.imagen = "https://endoback.prbs.li/" + this.imagenes[0].urlImagen;
      console.log(this.imagenes);
      this.imagenes.forEach(async (e, i) => {
        this.imagen = await "";
        this.imagen = (await "https://endoback.prbs.li/") + e.urlImagen;

        let idProducto = null;
        let imagenProducto = null;

        const xhr = new XMLHttpRequest();
        xhr.onload = async () => {
          var reader = new FileReader();
          reader.onloadend = async function () {
            idProducto = await e._id;
            imagenProducto = await reader.result;

            const response = await axios.put(
              `https://endoback.prbs.li/api/products/aws/${idProducto}`,
              { img: imagenProducto }
            );
            console.log(response);
            this.imagenesSubidas = i + 1;
          };
          reader.readAsDataURL(xhr.response);
        };
        xhr.open("GET", this.imagen);
        xhr.responseType = "blob";
        xhr.send();
      });
    },
    async getProducts() {
      try {
        const { data } = await axios.get(
          "https://endoback.prbs.li/api/products/stores/list"
        );
        this.imagenes = data;
      } catch (error) {
        console.warn(error.response);
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
