<template>
  <canvas id="canvas" width="1000" height="1000"></canvas>
</template>

<script>
export default {
  methods: {
    draw() {
      const ctx = document.getElementById("canvas").getContext("2d");
      for (var i = 0; i < 20; i++) {
        for (var j = 0; j < 20; j++) {
          ctx.strokeStyle = "rgb(157,157,157, 128)";
          ctx.strokeRect(j * 50, i * 50, 50, 50);
        }
      }
    },
  },
  mounted() {
    this.draw();
  },
};
</script>
<style scoped>
canvas {
  border: 1px solid rgb(157, 157, 157, 128);
}
</style>
