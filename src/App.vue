<template>
  <div class="container">
    <button v-on:click="draw">Inicia</button>
    <tabuleiro-canvas></tabuleiro-canvas>
  </div>
</template>

<script>
import TabuleiroCanvas from "./components/TabuleiroCanvas.vue";

export default {
  components: {
    TabuleiroCanvas,
  },
  methods: {
    localizaMouse(event) {
      const canvas = document.getElementById("canvas");
      const bounding = canvas.getBoundingClientRect();
      const x = event.clientX - bounding.left;
      const y = event.clientY - bounding.top;
      console.log(x, y);
    },
  },
  mounted() {
    const canvas = document.getElementById("canvas");
    canvas.addEventListener("click", (event) => this.localizaMouse(event));
  },
};
</script>

<style>
.container {
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
