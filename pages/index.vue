<script setup>
import { ref, onMounted, nextTick } from "vue";
import p5 from "p5";

const canvasRef = ref(null);

onMounted(async () => {
  if (process.client) {
    await nextTick(); // DOM が確実にレンダリングされるまで待つ
    new p5((p) => {
      let particles = [];

      p.setup = () => {
        p.createCanvas(p.windowWidth, p.windowHeight).parent(canvasRef.value);
        for (let i = 0; i < 100; i++) {
          particles.push(new Particle(p));
        }
      };

      p.draw = () => {
        p.background(20, 20, 40);
        particles.forEach((particle) => {
          particle.update();
          particle.show();
        });
      };

      class Particle {
        constructor(p) {
          this.p = p;
          this.x = p.random(p.width);
          this.y = p.random(p.height);
          this.vx = p.random(-1, 1);
          this.vy = p.random(-1, 1);
          this.size = p.random(2, 5);
        }

        update() {
          this.x += this.vx;
          this.y += this.vy;
          if (this.x > this.p.width || this.x < 0) this.vx *= -1;
          if (this.y > this.p.height || this.y < 0) this.vy *= -1;
        }

        show() {
          this.p.noStroke();
          this.p.fill(255, 150);
          this.p.ellipse(this.x, this.y, this.size);
        }
      }
    }, canvasRef.value);
  }
});
</script>

<template>
  <div class="container">
    <div ref="canvasRef" class="canvas-container"></div>
    <div class="content">
      <h1>Welcome to My Portfolio</h1>
      <p>This is my introduction page built with Vue.js & p5.js</p>
    </div>
  </div>
</template>

<style scoped>
.container {
  position: relative;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}

.canvas-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
}

.content {
  text-align: center;
  color: white;
  z-index: 2;
  font-family: Arial, sans-serif;
}

h1 {
  font-size: 3rem;
}

p {
  font-size: 1.5rem;
}
</style>
