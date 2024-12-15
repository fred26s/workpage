<script setup>
import { onMounted, onUnmounted, ref, reactive } from "vue";

const mouseX = ref(0);
const mouseY = ref(0);
const stars = ref([]);
const nebulas = ref([]);
const circles = ref([]);
const numbers = ref([]);

const updateMousePosition = (event) => {
  mouseX.value = event.clientX;
  mouseY.value = event.clientY;
};

onMounted(() => {
  // 初始化 stars
  for (let i = 0; i < 20; i++) {
    stars.value.push({
      left: Math.random() * 100 + "%",
      top: Math.random() * 100 + "%",
      animationDelay: Math.random() * 5 + "s",
    });
  }

  // 初始化 nebulas
  for (let i = 0; i < 3; i++) {
    nebulas.value.push({
      left: Math.random() * 100 + "%",
      top: Math.random() * 100 + "%",
      hue: Math.random() * 360 + "deg",
    });
  }

  // 初始化 circles
  for (let i = 0; i < 5; i++) {
    circles.value.push({
      size: i * 20 + "vw",
      duration: i * 10 + "s",
      delay: i * -2 + "s",
      opacity: 0.1 - i * 0.02,
    });
  }

  // 初始化 numbers
  for (let i = 0; i < 30; i++) {
    numbers.value.push({
      left: Math.random() * 100 + "%",
      top: Math.random() * 100 + "%",
      animationDelay: Math.random() * 5 + "s",
      animationDuration: Math.random() * 5 + 10 + "s",
      symbol: ["＄", "€", "₿", "Ξ"][Math.floor(Math.random() * 4)],
    });
  }

  document.addEventListener("mousemove", updateMousePosition);
});

onUnmounted(() => {
  document.removeEventListener("mousemove", updateMousePosition);
});
</script>

<template>
  <div
    class="relative min-h-screen overflow-hidden bg-gradient-to-br from-gray-900 via-purple-900 to-blue-900"
    @mousemove="updateMousePosition"
  >
    <!-- 背景网格 -->
    <div class="absolute inset-0 bg-grid opacity-10"></div>

    <!-- 动态背景元素 -->
    <div class="absolute inset-0 overflow-hidden">
      <div
        v-for="star in stars"
        :key="star"
        class="star"
        :style="{
          left: star.left,
          top: star.top,
          animationDelay: star.animationDelay,
        }"
      ></div>
      <div
        v-for="nebula in nebulas"
        :key="nebula"
        class="nebula"
        :style="{
          left: nebula.left,
          top: nebula.top,
          '--hue': nebula.hue,
        }"
      ></div>
    </div>

    <!-- 动态背景圆圈 -->
    <div
      v-for="circle in circles"
      :key="circle"
      class="animated-circle"
      :style="{
        '--size': circle.size,
        '--duration': circle.duration,
        '--delay': circle.delay,
        '--opacity': circle.opacity,
      }"
    ></div>

    <!-- 动态光晕效果 -->
    <div
      class="glow"
      :style="{ left: mouseX + 'px', top: mouseY + 'px' }"
    ></div>

    <!-- 主要内容 -->
    <div
      class="relative z-10 flex flex-col items-center justify-center min-h-screen p-4"
    >
      <h1
        class="text-8xl font-extrabold text-transparent bg-clip-text bg-gradient-to-r from-cyan-300 via-purple-300 to-pink-300 mb-4 tracking-tighter hover-shadow"
      >
        1dollar
      </h1>
      <p
        class="text-2xl text-gray-300 mb-12 text-center max-w-2xl font-light tracking-wide leading-relaxed hover-shadow"
      >
        Redefine your digital journey with unparalleled value
      </p>
      <!-- 粒子效果 -->
      <div v-if="showParticles" class="particles">
        <div
          v-for="n in 30"
          :key="n"
          class="particle"
          :style="{
            '--x': `${Math.random() * 300 - 150}px`,
            '--y': `${Math.random() * 300 - 150}px`,
            '--duration': `${Math.random() * 1.5 + 0.5}s`,
            '--delay': `${Math.random() * 0.5}s`,
            '--size': `${Math.random() * 5 + 2}px`,
            '--color': `hsl(${Math.random() * 60 + 180}, 100%, 70%)`,
          }"
        ></div>
      </div>

      <button
        class="relative px-8 py-4 text-lg font-medium text-white overflow-hidden rounded-full transition-all duration-300 ease-out focus:outline-none button-shadow z-10"
        @mouseenter="showParticles = true"
        @mouseleave="showParticles = false"
        @click="console.log('Looking forward to it.')"
      >
        <span
          class="absolute inset-0 w-full h-full bg-gradient-to-r from-purple-600 to-pink-600 opacity-75"
        ></span>
        <span
          class="absolute inset-0 w-full h-full bg-gradient-to-r from-cyan-500 to-blue-500 opacity-0 transition-opacity duration-300 ease-out group-hover:opacity-75"
        ></span>
        <span class="relative">LOOKING FORWARD</span>
      </button>
    </div>

    <!-- 浮动的数字矩阵 -->
    <div class="absolute inset-0 overflow-hidden pointer-events-none">
      <div
        v-for="number in numbers"
        :key="number"
        class="floating-number"
        :style="{
          left: number.left,
          top: number.top,
          animationDelay: number.animationDelay,
          animationDuration: number.animationDuration,
        }"
      >
        {{ number.symbol }}
      </div>
    </div>
  </div>
</template>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Syncopate:wght@400;700&family=Montserrat:wght@300;400;700&display=swap");

body {
  font-family: "Montserrat", sans-serif;
}

h1 {
  font-family: "Syncopate", sans-serif;
}

.bg-grid {
  background-image: linear-gradient(
      to right,
      rgba(255, 255, 255, 0.05) 1px,
      transparent 1px
    ),
    linear-gradient(to bottom, rgba(255, 255, 255, 0.05) 1px, transparent 1px);
  background-size: 50px 50px;
}

.glow {
  position: absolute;
  width: 300px;
  height: 300px;
  background: radial-gradient(
    circle,
    rgba(0, 255, 255, 0.2) 0%,
    rgba(0, 255, 255, 0) 70%
  );
  border-radius: 50%;
  pointer-events: none;
  transform: translate(-50%, -50%);
  filter: blur(20px);
  transition: 0.1s ease;
}

.animated-circle {
  position: absolute;
  width: var(--size);
  height: var(--size);
  border: 2px solid rgba(255, 255, 255, var(--opacity));
  border-radius: 50%;
  animation: pulse var(--duration) ease-in-out infinite alternate,
    rotate calc(var(--duration) * 2) linear infinite;
  animation-delay: var(--delay);
  background: linear-gradient(
    45deg,
    rgba(255, 255, 255, 0.05) 0%,
    rgba(255, 255, 255, 0) 100%
  );
}

.animated-circle::before {
  content: "";
  position: absolute;
  top: 10%;
  left: 10%;
  width: 20%;
  height: 20%;
  background-color: rgba(255, 255, 255, 0.1);
  border-radius: 50%;
  filter: blur(5px);
}

@keyframes pulse {
  0% {
    transform: scale(1);
    opacity: var(--opacity);
  }
  100% {
    transform: scale(1.1);
    opacity: calc(var(--opacity) * 0.8);
  }
}

@keyframes rotate {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

.particles {
  position: relative;
}

.particle {
  position: absolute;
  width: var(--size);
  height: var(--size);
  background: var(--color);
  border-radius: 50%;
  animation: explode var(--duration) ease-out var(--delay) forwards;
}

@keyframes explode {
  0% {
    transform: translate(0, 0);
    opacity: 1;
  }
  100% {
    transform: translate(var(--x), var(--y));
    opacity: 0;
  }
}

.floating-number {
  position: absolute;
  font-size: 24px;
  color: rgba(0, 255, 255, 0.3);
  opacity: 0;
  animation: float-number infinite linear;
}

@keyframes float-number {
  0% {
    opacity: 0;
  }
  50% {
    opacity: 1;
  }
  to {
    transform: translateY(-100vh);
    opacity: 0;
  }
  100% {
    opacity: 0;
  }
}

.hover-shadow {
  transition: text-shadow 0.3s ease, transform 0.3s ease;
}

.hover-shadow:hover {
  text-shadow: 0 0 10px rgba(0, 255, 255, 0.5), 0 0 20px rgba(0, 255, 255, 0.3),
    0 0 30px rgba(0, 255, 255, 0.2);
  transform: translateY(-2px);
}

.button-shadow {
  transition: box-shadow 0.3s ease, transform 0.3s ease;
}

.button-shadow:hover {
  box-shadow: 0 0 15px rgba(0, 255, 255, 0.3), 0 0 30px rgba(0, 255, 255, 0.2);
  transform: translateY(-1px);
}

.star {
  position: absolute;
  width: 2px;
  height: 2px;
  background: white;
  border-radius: 50%;
  animation: twinkle 4s infinite alternate;
}

@keyframes twinkle {
  0% {
    opacity: 0.2;
    transform: scale(1);
  }
  100% {
    opacity: 1;
    transform: scale(1.5);
  }
}

.nebula {
  position: absolute;
  width: 100px;
  height: 100px;
  background: radial-gradient(
    circle,
    hsla(var(--hue), 100%, 70%, 0.2) 0%,
    transparent 70%
  );
  filter: blur(20px);
  animation: pulse 10s infinite alternate;
}

@keyframes pulse {
  0% {
    transform: scale(1);
    opacity: 0.5;
  }
  100% {
    transform: scale(1.5);
    opacity: 0.8;
  }
}
</style>
