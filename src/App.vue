<template>
  <div class="container">
    <Transition name="fade" mode="out-in">
      <!-- NORMAL SCREEN -->
      <div v-if="!accepted" key="normal">
        <div class="bear-wrapper">
          <img :src="bear" alt="Cute bear" class="bear" />
          <p class="waiting-text">waiting...</p>
        </div>

        <h1 class="question">Will you be my valentine?</h1>

        <div class="buttons">
          <button class="yes-btn" @mouseenter="onYesHover" @click="onYesClick">
            Yes
          </button>

          <button
            v-if="showNo"
            class="no-btn"
            :class="{ escaped: noEscaped }"
            :style="noEscaped ? noStyle : {}"
            @mouseenter="moveNoButton"
          >
            No
          </button>
        </div>

        <p v-if="showNote" class="note">Please click on yes 🐻❤️</p>
      </div>

      <!-- CELEBRATION SCREEN -->
      <div v-else key="celebration" class="celebration">
        <img
          src="./assets/bears-hug.gif"
          alt="Bears hugging"
          class="hug-image"
        />

        <h1 class="celebrate-text">Happy Valentine’s Day 💖</h1>

        <p class="thank-you">
          Thank you <strong>Amishi</strong> for being the Valentine of
          <strong>Anshul</strong> 💕
        </p>

        <div v-if="showConfetti" class="confetti"></div>
      </div>
    </Transition>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import bear from "./assets/bear.png";

export default defineComponent({
  name: "App",

  setup() {
    const noStyle = ref({
      left: "calc(50% + 110px)",
      top: "calc(50% + 40px)"
    });

    const accepted = ref(false);
    const noEscaped = ref(false);
    const showNo = ref(true);
    const showNote = ref(false);
    const showConfetti = ref(false);

    const moveNoButton = () => {
      noEscaped.value = true;

      const padding = 100;

      const x = Math.random() * (window.innerWidth - padding * 2) + padding;
      const y = Math.random() * (window.innerHeight - padding * 2) + padding;

      noStyle.value = {
        left: `${x}px`,
        top: `${y}px`
      };
    };

    const onYesHover = () => {
      showNo.value = false;
      showNote.value = true;
    };

    const onYesClick = () => {
      accepted.value = true;

      // let celebration screen mount first
      setTimeout(() => {
        showConfetti.value = true;
      }, 100);
    };

    return {
      bear,
      noStyle,
      showNo,
      showNote,
      accepted,
      noEscaped,
      showConfetti,
      moveNoButton,
      onYesHover,
      onYesClick
    };
  }
});
</script>

<style scoped>
.container {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  font-family: system-ui, sans-serif;
  text-align: center;
}

.bear {
  width: 180px;
}

.waiting-text {
  margin-top: 6px;
  font-size: 14px;
  color: #777;
}

.bear-wrapper {
  animation: float 3s ease-in-out infinite;
}

@keyframes float {
  0% {
    transform: translateY(0);
  }

  50% {
    transform: translateY(-8px);
  }

  100% {
    transform: translateY(0);
  }
}

.question {
  margin-top: 30px;
  font-size: 26px;
  font-weight: 600;
  color: #333;
}

.buttons {
  margin-top: 30px;
  display: flex;
  gap: 30px;
  justify-content: center;
}

button {
  padding: 12px 30px;
  font-size: 18px;
  border-radius: 25px;
  border: none;
  cursor: pointer;
}

.yes-btn {
  background-color: #4caf50;
  color: white;
  margin-right: 20px;
}

.no-btn {
  background-color: #f44336;
  color: white;
  padding: 12px 30px;
  font-size: 18px;
  border-radius: 25px;
  border: none;
  cursor: pointer;
}

.no-btn.escaped {
  position: fixed;
  z-index: 1000;
  transition:
    left 0.15s ease,
    top 0.15s ease;
}

.note {
  margin-top: 25px;
  font-size: 18px;
  color: #555;
  animation: pulse 1.5s ease-in-out infinite;
}

@keyframes pulse {
  0% {
    transform: scale(1);
  }

  50% {
    transform: scale(1.05);
  }

  100% {
    transform: scale(1);
  }
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.4s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.confetti {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 999;

  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 64 64'%3E%3Cg transform='translate(16,16) scale(0.5)'%3E%3Cpath fill='%23ff4081' d='M23.6,0c-3.4,0-6.4,2.1-7.6,5.1C14.8,2.1,11.8,0,8.4,0C3.8,0,0,3.8,0,8.4c0,9.4,16,21.2,16,21.2s16-11.8,16-21.2C32,3.8,28.2,0,23.6,0z'/%3E%3C/g%3E%3C/svg%3E");

  background-repeat: repeat;
  background-size: 100px 100px;

  animation: heartFall 25s linear infinite;
}

@keyframes heartFall {
  from {
    background-position: 0 -100%;
  }
  to {
    background-position: 0 100%;
  }
}
</style>
