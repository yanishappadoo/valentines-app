<template>
  <div class="container">
    <h1>💖 Will you be my Valentine? 💖</h1>

    <div class="buttons">
      <!-- YES button -->
      <button
        class="yes"
        :style="{ transform: `scale(${yesScale})`, left: yesX + 'px', top: yesY + 'px' }"
        @click="yesClicked"
      >
        YES 💘
      </button>

      <!-- NO button -->
     <button
      class="no"
      :style="{ left: noX + 'px', top: noY + 'px' }"
      @mouseenter="moveNo"
      @touchstart.prevent="moveNo"
    >
      NO 😢
    </button>

    </div>

    <p v-if="accepted" class="success">
      🥰 Yay! Happy Valentine’s Day Trishina! 🥰
    </p>

    <!-- Floating hearts -->
    <div v-for="heart in hearts" :key="heart.id" 
         class="heart" 
         :style="{ left: heart.x + 'px', animationDuration: heart.duration + 's' }">
      ❤️
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

const yesScale = ref(1);
const yesX = ref(150);
const yesY = ref(150);
const noX = ref(300);
const noY = ref(150);
const accepted = ref(false);
const hearts = ref([]);

// Ensure buttons stay within viewport
function randomPosition(width = 100, height = 50) {
  return {
    x: Math.random() * (window.innerWidth - width),
    y: Math.random() * (window.innerHeight - height),
  };
}

// Move YES button randomly
function moveYes() {
  const pos = randomPosition(100 * yesScale.value, 50 * yesScale.value);
  yesX.value = pos.x;
  yesY.value = pos.y;
}

// Move NO button randomly and make YES bigger
function moveNo() {
  const pos = randomPosition(100 * yesScale.value, 50 * yesScale.value);
  noX.value = pos.x;
  noY.value = pos.y;
  yesScale.value += 0.2;
  moveYes();
}

// YES clicked
function yesClicked() {
  accepted.value = true;
}

// Floating hearts
function createHeart() {
  const id = Date.now() + Math.random();
  hearts.value.push({
    id,
    x: Math.random() * window.innerWidth,
    duration: 3 + Math.random() * 3
  });
  setTimeout(() => {
    hearts.value = hearts.value.filter(h => h.id !== id);
  }, 6000);
}

onMounted(() => {
  setInterval(createHeart, 500);
  setInterval(moveYes, 2500); // YES slowly floats too
});
</script>

<style>
body {
  margin: 0;
  font-family: "Segoe UI", sans-serif;
  overflow: hidden;
  background: url('/car-bg.jpeg') no-repeat center center fixed;
  background-size: contain;
  color: white;
  text-shadow: 1px 1px 5px rgba(0,0,0,0.7);
}

.container {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  position: relative;
}

h1 {
  margin-bottom: 40px;
}

.buttons {
  position: relative;
  width: 100%;
  height: 200px;
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
  background-color: rgba(255, 77, 109, 0.8);
  color: white;
}

.no {
  background-color: rgba(173, 181, 189, 0.8);
  color: #333;
}

.success {
  margin-top: 30px;
  font-size: 22px;
  color: #b0003a;
}

/* Floating hearts */
.heart {
  position: fixed;
  bottom: -50px;
  font-size: 24px;
  pointer-events: none;
  animation: float 6s linear forwards;
}

@keyframes float {
  0% { transform: translateY(0) scale(1); opacity: 1; }
  100% { transform: translateY(-100vh) scale(1.5); opacity: 0; }
}
</style>
