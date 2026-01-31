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
  </div>
</template>

<script setup>
import { ref } from "vue";

const yesScale = ref(1);
const accepted = ref(false);

const noX = ref(0);
const noY = ref(0);

function moveNo() {
  // Move NO button randomly
  noX.value = Math.random() * 200 - 100;
  noY.value = Math.random() * 120 - 60;

  // Grow YES button
  yesScale.value += 0.2;
}

function yesClicked() {
  accepted.value = true;
}
</script>

<style>
body {
  margin: 0;
  background: linear-gradient(135deg, #ffafbd, #ffc3a0);
  font-family: "Segoe UI", sans-serif;
}

.container {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
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
</style>
