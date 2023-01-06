<template>
  <canvas id="canvas" width="1000" height="1000"></canvas>
</template>

<script>
export default {
  data() {
    return {
      quadradosPintados: {
        0: [],
        1: [],
        2: [],
        3: [],
        4: [],
        5: [],
        6: [],
        7: [],
        8: [],
        9: [],
        10: [],
        11: [],
        12: [],
        13: [],
        14: [],
        15: [],
        16: [],
        17: [],
        18: [],
        19: [],
      },
    };
  },
  methods: {
    desenhaTabuleiro() {
      const ctx = document.getElementById("canvas").getContext("2d");
      for (var i = 0; i < 20; i++) {
        for (var j = 0; j < 20; j++) {
          ctx.strokeStyle = "rgb(157,157,157, 128)";
          ctx.strokeRect(j * 50, i * 50, 50, 50);
        }
      }
    },
    pintaQuadrado(x, y) {
      const ctx = document.getElementById("canvas").getContext("2d");
      if (this.quadradosPintados[x].includes(y)) {
        ctx.clearRect(x * 50, y * 50, 50, 50);
        ctx.strokeRect(x * 50, y * 50, 50, 50);
        this.quadradosPintados[x] = this.quadradosPintados[x].replace(y, "");
      } else {
        ctx.fillStyle = "rgb(0, 0, 0, 128)";
        ctx.fillRect(x * 50, y * 50, 50, 50);
        this.quadradosPintados[x] += y;
      }
    },
  },
  mounted() {
    this.desenhaTabuleiro();
  },
};
</script>
<style scoped>
canvas {
  border: 1px solid rgb(157, 157, 157, 128);
}
</style>
