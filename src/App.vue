<template>
  <div class="relative min-h-screen overflow-hidden bg-gradient-to-br from-gray-900 via-purple-900 to-blue-900"
       @mousemove="updateMousePosition">
    <!-- 背景网格 -->
    <div class="absolute inset-0 bg-grid opacity-10"></div>

    <!-- 动态背景圆圈 -->
    <div v-for="n in 5" :key="n" class="animated-circle"
         :style="{
           '--size': `${n * 20}vw`,
           '--duration': `${n * 10}s`,
           '--delay': `${n * -2}s`,
           '--opacity': `${0.1 - n * 0.02}`
         }">
    </div>

    <!-- 动态光晕效果 -->
    <div class="glow" :style="{ left: mouseX + 'px', top: mouseY + 'px' }"></div>

    <!-- 主要内容 -->
    <div class="relative z-10 flex flex-col items-center justify-center min-h-screen p-4">
      <h1 class="text-8xl font-extrabold text-transparent bg-clip-text bg-gradient-to-r from-cyan-300 via-purple-300 to-pink-300 mb-4 tracking-tighter hover-shadow">
        1dollar
      </h1>
      <p class="text-2xl text-gray-300 mb-12 text-center max-w-2xl font-light tracking-wide leading-relaxed hover-shadow">
        Redefine your digital journey with unparalleled value
      </p>
      
      <button class="group relative px-8 py-4 text-lg font-medium text-white overflow-hidden rounded-full 
                     transition-all duration-300 ease-out hover:scale-105 focus:outline-none button-shadow"
              @mouseover="showParticles = true"
              @mouseleave="showParticles = false">
        <span class="absolute inset-0 w-full h-full transition-all duration-300 ease-out transform translate-x-0 -skew-x-12 bg-gradient-to-r from-purple-500 to-pink-500 group-hover:bg-gradient-to-r group-hover:from-cyan-400 group-hover:to-blue-500"></span>
        <span class="absolute inset-0 w-full h-full transition-all duration-300 ease-out transform skew-x-12 bg-gradient-to-r from-purple-700 to-pink-700 group-hover:bg-gradient-to-r group-hover:from-cyan-600 group-hover:to-blue-700 opacity-0 group-hover:opacity-100"></span>
        <span class="relative">Explore the Future</span>
      </button>

      <!-- 粒子效果 -->
      <div v-if="showParticles" class="particles">
        <div v-for="n in 30" :key="n" class="particle"
             :style="{ 
               '--x': `${Math.random() * 300 - 150}px`,
               '--y': `${Math.random() * 300 - 150}px`,
               '--duration': `${Math.random() * 1.5 + 0.5}s`,
               '--delay': `${Math.random() * 0.5}s`,
               '--size': `${Math.random() * 5 + 2}px`,
               '--color': `hsl(${Math.random() * 60 + 180}, 100%, 70%)`
             }">
        </div>
      </div>
    </div>

    <!-- 浮动的数字矩阵 -->
    <div class="absolute inset-0 overflow-hidden pointer-events-none">
      <div v-for="n in 20" :key="n" class="floating-number"
           :style="{
             left: `${Math.random() * 100}%`,
             top: `${Math.random() * 100}%`,
             animationDuration: `${Math.random() * 10 + 5}s`,
             animationDelay: `${Math.random() * 5}s`
           }">
        {{ Math.random() > 0.5 ? '0' : '1' }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const mouseX = ref(0);
const mouseY = ref(0);
const showParticles = ref(false);

const updateMousePosition = (event) => {
  mouseX.value = event.clientX;
  mouseY.value = event.clientY;
};

onMounted(() => {
  window.addEventListener('mousemove', updateMousePosition);
});

onUnmounted(() => {
  window.removeEventListener('mousemove', updateMousePosition);
});
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Syncopate:wght@400;700&family=Montserrat:wght@300;400;700&display=swap');

body {
  font-family: 'Montserrat', sans-serif;
}

h1 {
  font-family: 'Syncopate', sans-serif;
}

.bg-grid {
  background-image: 
    linear-gradient(to right, rgba(255,255,255,.05) 1px, transparent 1px),
    linear-gradient(to bottom, rgba(255,255,255,.05) 1px, transparent 1px);
  background-size: 50px 50px;
}

.glow {
  position: absolute;
  width: 300px;
  height: 300px;
  background: radial-gradient(circle, rgba(0,255,255,0.2) 0%, rgba(0,255,255,0) 70%);
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
  border: 2px solid rgba(255,255,255,var(--opacity));
  border-radius: 50%;
  animation: pulse var(--duration) ease-in-out infinite alternate,
             rotate calc(var(--duration) * 2) linear infinite;
  animation-delay: var(--delay);
}

@keyframes pulse {
  0% { transform: scale(1); opacity: var(--opacity); }
  100% { transform: scale(1.1); opacity: calc(var(--opacity) * 0.8); }
}

@keyframes rotate {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

.particles {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 300px;
  height: 300px;
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
  color: rgba(0,255,255,0.3);
  animation: float-up linear infinite;
}

@keyframes float-up {
  from { transform: translateY(0); opacity: 0; }
  50% { opacity: 1; }
  to { transform: translateY(-100vh); opacity: 0; }
}

.hover-shadow {
  transition: text-shadow 0.3s ease, transform 0.3s ease;
}

.hover-shadow:hover {
  text-shadow: 0 0 10px rgba(0, 255, 255, 0.5), 0 0 20px rgba(0, 255, 255, 0.3), 0 0 30px rgba(0, 255, 255, 0.2);
  transform: translateY(-2px);
}

.button-shadow {
  transition: box-shadow 0.3s ease, transform 0.3s ease;
}

.button-shadow:hover {
  box-shadow: 0 0 20px rgba(0, 255, 255, 0.5), 0 0 40px rgba(0, 255, 255, 0.3);
  transform: translateY(-2px);
}
</style>

