<template>
  <div class="container">
    <div class="options">
      <button v-on:click="iniciaJogo" :disabled="jogando">Iniciar</button>
      <button v-on:click="pausaJogo" :disabled="!jogando">Pausar</button>
      <button v-on:click="reiniciaTudo">Resetar</button>
    </div>
    <tabuleiro-canvas ref="TabuleiroCanvas"></tabuleiro-canvas>
    <div class="options">
      <button v-on:click="alteraVelocidade(true)">
        Aumentar Velocidade em 1x
      </button>
      <p>Velocidade: {{ velocidade }}x</p>
      <button v-on:click="alteraVelocidade(false)">
        Diminutir Velocidade em 1x
      </button>
    </div>
  </div>
</template>

<script>
import TabuleiroCanvas from "./components/TabuleiroCanvas.vue";

export default {
  data() {
    return {
      x: 0,
      y: 0,
      jogando: false,
      velocidade: 1,
    };
  },
  components: {
    TabuleiroCanvas,
  },
  methods: {
    localizaMouse(event) {
      if (!this.jogando) {
        const canvas = document.getElementById("canvas");
        const bounding = canvas.getBoundingClientRect();
        this.x = Math.floor((event.clientX - bounding.left) / 50);
        this.y = Math.floor((event.clientY - bounding.top) / 50);
        this.$refs.TabuleiroCanvas.pintaQuadrado(this.x, this.y);
      }
    },
    iniciaJogo() {
      this.jogando = true;
      this.$refs.TabuleiroCanvas.logicaJogo(this.jogando);
    },
    pausaJogo() {
      this.jogando = false;
      this.$refs.TabuleiroCanvas.logicaJogo(this.jogando);
    },
    reiniciaTudo() {
      this.jogando = false;
      this.$refs.TabuleiroCanvas.reseta(true);
    },
    alteraVelocidade(aumenta) {
      let velocidade = this.$refs.TabuleiroCanvas.velocidade;
      aumenta ? (velocidade = velocidade / 2) : (velocidade = velocidade * 2);
      aumenta ? (this.velocidade += 1) : (this.velocidade -= 1);
      this.$refs.TabuleiroCanvas.velocidade = velocidade;
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
  gap: 50px;
}

.options {
  display: flex;
  flex-direction: column;
  gap: 25px;
  align-items: center;
}
</style>
