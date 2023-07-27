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
      dias: 0,
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
        this.mataCelula(y, x);
      } else {
        this.criaCelula(y, x);
      }
    },
    logicaJogo(jogando) {
      this.dias++;
      this.jogando = jogando;
      if (jogando) {
        for (var y in this.quadradosPintados) {
          var coluna = Number(y);
          if (this.quadradosPintados[coluna].length > 0) {
            for (var x in this.quadradosPintados[coluna]) {
              var linha = this.quadradosPintados[coluna][x];
              var vizinhos = this.contaVizinhos(linha, coluna);
              this.verificaSeNasce(linha, coluna, vizinhos);
              if (vizinhos <= 1 || vizinhos >= 4) {
                this.vaiMorrer[coluna].push(linha);
              }
            }
          }
        }
        this.verificaListas(this.vaiMorrer, false);
        this.verificaListas(this.vaiNascer, true);
        this.reseta(false);
        setTimeout(() => {
          this.logicaJogo(this.jogando);
        }, 1500);
      }
    },
    verificaListas(lista, criar) {
      for (var numeroColuna in lista) {
        if (lista[numeroColuna].length > 0) {
          var coluna = Number(numeroColuna);
          for (var n in lista[numeroColuna]) {
            var linha = lista[numeroColuna][n];
            criar === true
              ? this.criaCelula(linha, coluna)
              : this.mataCelula(linha, coluna);
          }
        }
      }
    },
    mataCelula(linha, coluna) {
      const ctx = document.getElementById("canvas").getContext("2d");
      ctx.clearRect(coluna * 50, linha * 50, 50, 50);
      ctx.strokeStyle = "rgb(157, 157, 157, 128)";
      ctx.strokeRect(coluna * 50, linha * 50, 50, 50);
      var indice = this.quadradosPintados[coluna].indexOf(linha);
      this.quadradosPintados[coluna].splice(indice, 1);
    },
    criaCelula(linha, coluna) {
      const ctx = document.getElementById("canvas").getContext("2d");
      ctx.fillStyle = "rgb(0, 0, 0, 128)";
      ctx.fillRect(coluna * 50, linha * 50, 50, 50);
      ctx.strokeStyle = "rgb(157,157,157, 128)";
      ctx.strokeRect(coluna * 50, linha * 50, 50, 50);
      if (!this.quadradosPintados[coluna].includes(linha)) {
        this.quadradosPintados[coluna].push(linha);
      }
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
    verificaSeNasce(linha, coluna, vizinhos) {
      if (vizinhos == 8) {
        return;
      } else if (coluna - 1 < 0) {
        if (linha + 1 < 9) {
          if (!this.quadradosPintados[coluna].includes(linha + 1)) {
            if (this.contaVizinhos(linha + 1, coluna) == 3) {
              this.vaiNascer[coluna].push(linha + 1);
            }
          }
          if (!this.quadradosPintados[coluna + 1].includes(linha + 1)) {
            if (this.contaVizinhos(linha + 1, coluna + 1) == 3) {
              this.vaiNascer[coluna + 1].push(linha + 1);
            }
          }
        }
        if (!this.quadradosPintados[coluna + 1].includes(linha)) {
          if (this.contaVizinhos(linha, coluna + 1) == 3) {
            this.vaiNascer[coluna + 1].push(linha);
          }
        }
        if (linha - 1 >= 0) {
          if (!this.quadradosPintados[coluna].includes(linha - 1)) {
            if (this.contaVizinhos(linha - 1, coluna) == 3) {
              this.vaiNascer[coluna].push(linha - 1);
            }
          }
          if (!this.quadradosPintados[coluna + 1].includes(linha - 1)) {
            if (this.contaVizinhos(linha - 1, coluna + 1) == 3) {
              this.vaiNascer[coluna + 1].push(linha - 1);
            }
          }
        }
      } else if (coluna + 1 > 9) {
        if (linha - 1 >= 0) {
          if (!this.quadradosPintados[coluna].includes(linha - 1)) {
            if (this.contaVizinhos(linha - 1, coluna) == 3) {
              this.vaiNascer[coluna].push(linha - 1);
            }
          }
          if (!this.quadradosPintados[coluna - 1].includes(linha - 1)) {
            if (this.contaVizinhos(linha - 1, coluna - 1) == 3) {
              this.vaiNascer[coluna - 1].push(linha - 1);
            }
          }
        }
        if (!this.quadradosPintados[coluna - 1].includes(linha)) {
          if (this.contaVizinhos(linha, coluna - 1) == 3) {
            this.vaiNascer[coluna - 1].push(linha);
          }
        }
        if (linha + 1 < 9) {
          if (!this.quadradosPintados[coluna - 1].includes(linha + 1)) {
            if (this.contaVizinhos(linha + 1, coluna - 1) == 3) {
              this.vaiNascer[coluna - 1].push(linha + 1);
            }
          }
          if (!this.quadradosPintados[coluna].includes(linha + 1)) {
            if (this.contaVizinhos(linha + 1, coluna) == 3) {
              this.vaiNascer[coluna].push(linha + 1);
            }
          }
        }
      } else {
        if (linha - 1 < 0) {
          if (!this.quadradosPintados[coluna - 1].includes(linha + 1)) {
            if (this.contaVizinhos(linha + 1, coluna - 1) == 3) {
              this.vaiNascer[coluna - 1].push(linha + 1);
            }
          }
          if (!this.quadradosPintados[coluna - 1].includes(linha)) {
            if (this.contaVizinhos(linha, coluna - 1) == 3) {
              this.vaiNascer[coluna - 1].push(linha);
            }
          }
          if (!this.quadradosPintados[coluna].includes(linha + 1)) {
            if (this.contaVizinhos(linha + 1, coluna) == 3) {
              this.vaiNascer[coluna].push(linha + 1);
            }
          }
          if (!this.quadradosPintados[coluna + 1].includes(linha + 1)) {
            if (this.contaVizinhos(linha + 1, coluna + 1) == 3) {
              this.vaiNascer[coluna + 1].push(linha + 1);
            }
          }
          if (!this.quadradosPintados[coluna + 1].includes(linha)) {
            if (this.contaVizinhos(linha, coluna + 1) == 3) {
              this.vaiNascer[coluna + 1].push(linha);
            }
          }
        } else if (linha + 1 > 9) {
          if (!this.quadradosPintados[coluna - 1].includes(linha - 1)) {
            if (this.contaVizinhos(linha - 1, coluna - 1) == 3) {
              this.vaiNascer[coluna - 1].push(linha - 1);
            }
          }
          if (!this.quadradosPintados[coluna - 1].includes(linha)) {
            if (this.contaVizinhos(linha, coluna - 1) == 3) {
              this.vaiNascer[coluna - 1].push(linha);
            }
          }
          if (!this.quadradosPintados[coluna].includes(linha - 1)) {
            if (this.contaVizinhos(linha - 1, coluna) == 3) {
              this.vaiNascer[coluna].push(linha - 1);
            }
          }
          if (!this.quadradosPintados[coluna + 1].includes(linha - 1)) {
            if (this.contaVizinhos(linha - 1, coluna + 1) == 3) {
              this.vaiNascer[coluna + 1].push(linha - 1);
            }
          }
          if (!this.quadradosPintados[coluna + 1].includes(linha)) {
            if (this.contaVizinhos(linha, coluna + 1) == 3) {
              this.vaiNascer[coluna + 1].push(linha);
            }
          }
        } else {
          if (!this.quadradosPintados[coluna - 1].includes(linha + 1)) {
            if (this.contaVizinhos(linha + 1, coluna - 1) == 3) {
              this.vaiNascer[coluna - 1].push(linha + 1);
            }
          }
          if (!this.quadradosPintados[coluna - 1].includes(linha - 1)) {
            if (this.contaVizinhos(linha - 1, coluna - 1) == 3) {
              this.vaiNascer[coluna - 1].push(linha - 1);
            }
          }
          if (!this.quadradosPintados[coluna - 1].includes(linha)) {
            if (this.contaVizinhos(linha, coluna - 1) == 3) {
              this.vaiNascer[coluna - 1].push(linha);
            }
          }
          if (!this.quadradosPintados[coluna].includes(linha + 1)) {
            if (this.contaVizinhos(linha + 1, coluna) == 3) {
              this.vaiNascer[coluna].push(linha + 1);
            }
          }
          if (!this.quadradosPintados[coluna].includes(linha - 1)) {
            if (this.contaVizinhos(linha - 1, coluna) == 3) {
              this.vaiNascer[coluna].push(linha - 1);
            }
          }
          if (!this.quadradosPintados[coluna + 1].includes(linha + 1)) {
            if (this.contaVizinhos(linha + 1, coluna + 1) == 3) {
              this.vaiNascer[coluna + 1].push(linha + 1);
            }
          }
          if (!this.quadradosPintados[coluna + 1].includes(linha - 1)) {
            if (this.contaVizinhos(linha - 1, coluna + 1) == 3) {
              this.vaiNascer[coluna + 1].push(linha - 1);
            }
          }
          if (!this.quadradosPintados[coluna + 1].includes(linha)) {
            if (this.contaVizinhos(linha, coluna + 1) == 3) {
              this.vaiNascer[coluna + 1].push(linha);
            }
          }
        }
      }
    },
    reseta(tudo) {
      this.vaiMorrer = [[], [], [], [], [], [], [], [], [], []];
      this.vaiNascer = [[], [], [], [], [], [], [], [], [], []];
      if (tudo) {
        this.dias = 0;
        this.quadradosPintados = [[], [], [], [], [], [], [], [], [], []];
        const ctx = document.getElementById("canvas").getContext("2d");
        ctx.reset();
        this.desenhaTabuleiro();
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
