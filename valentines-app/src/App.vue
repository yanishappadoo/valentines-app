<template>
  <!-- Loading Screen -->
  <transition name="fade">
    <div v-if="loading" class="loading-screen">
      <div class="loading-content">
        <img src="/newpic.jpeg" class="loading-image" />
        <div class="loading-text">
          💌 Loading your surprise... 💌
        </div>
      </div>
    </div>
  </transition>

  <!-- GIF Screen -->
  <transition name="fade">
    <div v-if="showGif" class="gif-screen">
      <img src="/gif.gif" class="gif-image" />

      <div class="gif-overlay"></div>

      <!-- Flying Roses -->
      <div
        v-for="rose in roses"
        :key="rose.id"
        class="rose"
        :style="{
          left: rose.x + 'px',
          fontSize: rose.size + 'px',
          animationDuration: rose.duration + 's'
        }"
      >
        🌹
      </div>

      <div class="romantic-text">
        💖 My precious girlfriend Trishina 💖 <br />
        You are the most beautiful part of my life. <br />
        Every moment with you feels magical ✨ <br />
        I love you endlessly.
      </div>
    </div>
  </transition>

  <!-- Valentine Page -->
  <div v-show="!loading && !showGif" class="container">
    <div class="valentine-card">
      <h1>💖 Will you be my Valentine? 💖</h1>

      <div class="buttons">
        <button
          class="yes"
          :style="{ transform: `scale(${yesScale})`, left: yesX + 'px', top: yesY + 'px' }"
          @click="yesClicked"
        >
          YES 💘
        </button>

        <button
          class="no"
          :style="{ left: noX + 'px', top: noY + 'px' }"
          @mouseenter="moveNo"
          @touchstart.prevent="moveNo"
        >
          NO 😢
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

const loading = ref(true);
const showGif = ref(false);

const yesScale = ref(1);
const yesX = ref(150);
const yesY = ref(100);
const noX = ref(300);
const noY = ref(100);

const roses = ref([]);

function randomPosition(width = 100, height = 50) {
  return {
    x: Math.random() * (window.innerWidth - width),
    y: Math.random() * (window.innerHeight - height),
  };
}

function moveYes() {
  const pos = randomPosition(100 * yesScale.value, 50 * yesScale.value);
  yesX.value = pos.x;
  yesY.value = pos.y;
}

function moveNo() {
  const pos = randomPosition(100, 50);
  noX.value = pos.x;
  noY.value = pos.y;
  yesScale.value += 0.2;
  moveYes();
}

function yesClicked() {
  showGif.value = true;
  setInterval(createRose, 400);
}

function createRose() {
  const id = Date.now() + Math.random();

  roses.value.push({
    id,
    x: Math.random() * window.innerWidth,
    size: 20 + Math.random() * 30,
    duration: 4 + Math.random() * 4,
  });

  setTimeout(() => {
    roses.value = roses.value.filter(r => r.id !== id);
  }, 8000);
}

onMounted(() => {
  setTimeout(() => {
    loading.value = false;
  }, 2000);
});
</script>

<style>
body {
  margin: 0;
  font-family: "Segoe UI", sans-serif;
  overflow: hidden;
}

/* Fade Animation */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 1s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

/* ---------------- LOADING ---------------- */
.loading-screen {
  position: fixed;
  inset: 0;
  background: black;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

.loading-content {
  text-align: center;
}

.loading-image {
  max-width: 300px;
  max-height: 400px;
  object-fit: contain;
  margin-bottom: 20px;
}

.loading-text {
  font-size: 28px;
  color: #ff4d6d;
  text-shadow: 0 0 20px rgba(255, 77, 109, 0.9);
  animation: blink 1.5s infinite;
}

@keyframes blink {
  0%, 50%, 100% { opacity: 1; }
  25%, 75% { opacity: 0.3; }
}

/* ---------------- VALENTINE PAGE ---------------- */
.container {
  height: 100vh;
  background: url("/car-bg.jpeg") no-repeat center center;
  background-size: cover;
  display: flex;
  justify-content: center;
  align-items: center;
}

.valentine-card {
  text-align: center;
  color: white;
  text-shadow: 0 0 15px rgba(255, 105, 180, 0.7);
  padding: 40px;
  border-radius: 20px;
  background: rgba(0,0,0,0.25); /* subtle background, no blur */
}

.buttons {
  position: relative;
  width: 500px;
  height: 200px;
  margin-top: 40px;
}

button {
  padding: 14px 28px;
  font-size: 18px;
  border-radius: 30px;
  border: none;
  cursor: pointer;
  position: absolute;
  transition: all 0.3s ease;
}

.yes {
  background-color: rgba(255, 77, 109, 0.95);
  color: white;
}

.no {
  background-color: rgba(173, 181, 189, 0.95);
  color: #333;
}

/* ---------------- GIF SCREEN ---------------- */
.gif-screen {
  position: fixed;
  inset: 0;
  background: black;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  z-index: 9999;
}

.gif-image {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
  z-index: 0;
}

.gif-overlay {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.45);
  z-index: 1;
}

.romantic-text {
  position: absolute;
  text-align: center;
  color: white;
  font-size: 28px;
  padding: 20px;
  text-shadow: 0 0 25px rgba(255, 105, 180, 0.9);
  animation: romanticFade 2s ease forwards;
  opacity: 0;
  z-index: 3;
  max-width: 80%;
}

@keyframes romanticFade {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* ---------------- ROSES ---------------- */
.rose {
  position: fixed;
  bottom: -50px;
  pointer-events: none;
  animation: floatRose linear forwards;
  z-index: 2;
}

@keyframes floatRose {
  0% {
    transform: translateY(0) rotate(0deg);
    opacity: 1;
  }
  100% {
    transform: translateY(-110vh) rotate(360deg);
    opacity: 0;
  }
}
</style>
