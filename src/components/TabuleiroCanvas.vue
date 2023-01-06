<template>
  <canvas id="canvas" width="1000" height="1000"></canvas>
</template>

<script>
export default {
  data() {
    return {
      jogando: Boolean,
      quadradosPintados: [
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
      ],
      vaiMorrer: [
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
        [],
      ],
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
      if (this.quadradosPintados[x].includes(y)) {
        this.mataCelula(x, y);
      } else {
        this.criaCelula(x, y);
      }
    },
    logicaJogo(jogando) {
      this.jogando = jogando;
      if (jogando) {
        for (var y in this.quadradosPintados) {
          var coluna = Number(y);
          if (this.quadradosPintados[coluna].length > 0) {
            for (var x in this.quadradosPintados[coluna]) {
              var linha = this.quadradosPintados[coluna][x];
              var vizinhos = this.contaVizinhos(linha, coluna);
              if (vizinhos <= 1 || vizinhos >= 4) {
                console.log("A celula morreu");
              } else {
                console.log("A celula vai viver!");
              }
              console.log("Essa celula tinha", vizinhos, "vizinhos");
            }
          }
        }
        setTimeout(() => {
          this.logicaJogo(this.jogando);
        }, 2000);
      }
    },
    contaVizinhos(linha, coluna) {
      var contadorVizinhos = 0;
      if (
        this.quadradosPintados[coluna + 1].length === 0 &&
        this.quadradosPintados[coluna - 1].length === 0 &&
        this.quadradosPintados[coluna].length === 1
      ) {
        return contadorVizinhos;
      } else if (this.quadradosPintados[coluna + 1].length > 0) {
        if (
          !this.quadradosPintados[coluna + 1].includes(linha + 1) &&
          !this.quadradosPintados[coluna + 1].includes(linha - 1) &&
          !this.quadradosPintados[coluna + 1].includes(linha)
        ) {
          return contadorVizinhos;
        } else {
          this.quadradosPintados[coluna + 1].includes(linha + 1)
            ? contadorVizinhos++
            : null;
          this.quadradosPintados[coluna + 1].includes(linha - 1)
            ? contadorVizinhos++
            : null;
          this.quadradosPintados[coluna + 1].includes(linha)
            ? contadorVizinhos++
            : null;
          return contadorVizinhos;
        }
      } else if (this.quadradosPintados[coluna - 1].length > 0) {
        if (
          !this.quadradosPintados[coluna - 1].includes(linha + 1) &&
          !this.quadradosPintados[coluna - 1].includes(linha - 1) &&
          !this.quadradosPintados[coluna - 1].includes(linha)
        ) {
          return contadorVizinhos;
        } else {
          this.quadradosPintados[coluna - 1].includes(linha + 1)
            ? contadorVizinhos++
            : null;
          this.quadradosPintados[coluna - 1].includes(linha - 1)
            ? contadorVizinhos++
            : null;
          this.quadradosPintados[coluna - 1].includes(linha)
            ? contadorVizinhos++
            : null;
          return contadorVizinhos;
        }
      } else if (
        !this.quadradosPintados[coluna].includes(linha - 1) &&
        !this.quadradosPintados[coluna].includes(linha + 1)
      ) {
        return contadorVizinhos;
      } else {
        this.quadradosPintados[coluna].includes(linha + 1)
          ? contadorVizinhos++
          : null;
        this.quadradosPintados[coluna].includes(linha - 1)
          ? contadorVizinhos++
          : null;
        return contadorVizinhos;
      }
    },
    mataCelula(coluna, linha) {
      const ctx = document.getElementById("canvas").getContext("2d");
      ctx.clearRect(coluna * 50, linha * 50, 50, 50);
      ctx.strokeStyle = "rgb(157, 157, 157, 128)";
      ctx.strokeRect(coluna * 50, linha * 50, 50, 50);
      var indice = this.quadradosPintados[coluna].indexOf(linha);
      this.quadradosPintados[coluna].splice(indice, 1);
    },
    criaCelula(coluna, linha) {
      const ctx = document.getElementById("canvas").getContext("2d");
      ctx.fillStyle = "rgb(0, 0, 0, 128)";
      ctx.fillRect(coluna * 50, linha * 50, 50, 50);
      this.quadradosPintados[coluna].push(linha);
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
