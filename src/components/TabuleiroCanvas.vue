<template>
  <canvas id="canvas" width="500" height="500"></canvas>
</template>

<script>
export default {
  data() {
    return {
      jogando: Boolean,
      quadradosPintados: [[], [], [], [], [], [], [], [], [], []],
      vaiMorrer: [[], [], [], [], [], [], [], [], [], []],
      vaiNascer: [[], [], [], [], [], [], [], [], [], []],
    };
  },
  methods: {
    desenhaTabuleiro() {
      const ctx = document.getElementById("canvas").getContext("2d");
      for (var i = 0; i < 10; i++) {
        for (var j = 0; j < 10; j++) {
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
              console.log(vizinhos, linha, coluna);
              if (vizinhos <= 1 || vizinhos >= 4) {
                this.vaiMorrer[coluna].push(linha);
              }
            }
          }
        }
        for (var morre in this.vaiMorrer) {
          if (this.vaiMorrer[morre].length > 0) {
            var colunaMorre = Number(morre);
            for (var z in this.vaiMorrer[morre]) {
              var linhaMorre = this.vaiMorrer[morre][z];
              this.mataCelula(colunaMorre, linhaMorre);
            }
          }
        }
        for (var nasce in this.vaiNascer) {
          if (this.vaiNascer[nasce].length > 0) {
            var colunaNasce = Number(nasce);
            for (var n in this.vaiNascer[nasce]) {
              var linhaNasce = this.vaiNascer[nasce][n];
              this.criaCelula(colunaNasce, linhaNasce);
            }
          }
        }
        setTimeout(() => {
          this.logicaJogo(this.jogando);
        }, 2000);
      }
    },
    mataCelula(coluna, linha) {
      const ctx = document.getElementById("canvas").getContext("2d");
      ctx.clearRect(coluna * 50, linha * 50, 50, 50);
      ctx.strokeStyle = "rgb(157, 157, 157, 128)";
      ctx.strokeRect(coluna * 50, linha * 50, 50, 50);
      var indice = this.quadradosPintados[coluna].indexOf(linha);
      this.quadradosPintados[coluna][indice] = -1;
    },
    criaCelula(coluna, linha) {
      const ctx = document.getElementById("canvas").getContext("2d");
      ctx.fillStyle = "rgb(0, 0, 0, 128)";
      ctx.fillRect(coluna * 50, linha * 50, 50, 50);
      this.quadradosPintados[coluna].push(linha);
    },
    contaVizinhos(linha, coluna) {
      var vizinhos = 0;
      if (coluna - 1 < 0) {
        this.quadradosPintados[coluna].includes(linha + 1) ? vizinhos++ : null;
        this.quadradosPintados[coluna].includes(linha - 1) ? vizinhos++ : null;
        this.quadradosPintados[coluna + 1].includes(linha + 1)
          ? vizinhos++
          : null;
        this.quadradosPintados[coluna + 1].includes(linha) ? vizinhos++ : null;
        this.quadradosPintados[coluna + 1].includes(linha - 1)
          ? vizinhos++
          : null;
      } else if (coluna + 1 > 9) {
        this.quadradosPintados[coluna].includes(linha + 1) ? vizinhos++ : null;
        this.quadradosPintados[coluna].includes(linha - 1) ? vizinhos++ : null;
        this.quadradosPintados[coluna - 1].includes(linha + 1)
          ? vizinhos++
          : null;
        this.quadradosPintados[coluna - 1].includes(linha) ? vizinhos++ : null;
        this.quadradosPintados[coluna - 1].includes(linha - 1)
          ? vizinhos++
          : null;
      } else {
        this.quadradosPintados[coluna].includes(linha + 1) ? vizinhos++ : null;
        this.quadradosPintados[coluna].includes(linha - 1) ? vizinhos++ : null;
        this.quadradosPintados[coluna + 1].includes(linha + 1)
          ? vizinhos++
          : null;
        this.quadradosPintados[coluna + 1].includes(linha) ? vizinhos++ : null;
        this.quadradosPintados[coluna + 1].includes(linha - 1)
          ? vizinhos++
          : null;
        this.quadradosPintados[coluna - 1].includes(linha + 1)
          ? vizinhos++
          : null;
        this.quadradosPintados[coluna - 1].includes(linha) ? vizinhos++ : null;
        this.quadradosPintados[coluna - 1].includes(linha - 1)
          ? vizinhos++
          : null;
      }
      return vizinhos;
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
