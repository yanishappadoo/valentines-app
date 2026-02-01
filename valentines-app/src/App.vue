<template>
  <div class="container">
    <h1>💖 Will you be my Valentine? 💖</h1>

    <div class="buttons">
      <button
        class="yes"
        :style="{ transform: `scale(${yesScale})` }"
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

    <p v-if="accepted" class="success">
      🥰 Yay! Happy Valentine’s Day! 🥰
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
const accepted = ref(false);

const noX = ref(0);
const noY = ref(0);

function moveNo() {
  noX.value = Math.random() * 200 - 100;
  noY.value = Math.random() * 120 - 60;
  yesScale.value += 0.2;
}

function yesClicked() {
  accepted.value = true;
}

// Hearts animation
const hearts = ref([]);

function createHeart() {
  const id = Date.now() + Math.random();
  hearts.value.push({
    id,
    x: Math.random() * window.innerWidth,
    duration: 3 + Math.random() * 3
  });
  // Remove heart after animation
  setTimeout(() => {
    hearts.value = hearts.value.filter(h => h.id !== id);
  }, 6000);
}

onMounted(() => {
  setInterval(createHeart, 500);
});
</script>

<style>
body {
  margin: 0;
  background: linear-gradient(135deg, #ffafbd, #ffc3a0);
  font-family: "Segoe UI", sans-serif;
  overflow-x: hidden;
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
  height: 150px;
  width: 300px;
}

button {
  padding: 14px 28px;
  font-size: 18px;
  border-radius: 30px;
  border: none;
  cursor: pointer;
  transition: all 0.2s ease;
}

.yes {
  background-color: #ff4d6d;
  color: white;
}

.no {
  position: absolute;
  background-color: #adb5bd;
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
