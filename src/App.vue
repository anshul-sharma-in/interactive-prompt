<template>
  <div class="container">
    <div class="bear-wrapper">
      <img :src="bear" alt="Cute bear" class="bear" />
      <p class="waiting-text">waiting...</p>
    </div>
    <h1 class="question">Will you be my valentine?</h1>
    <div class="buttons">
      <button class="yes-btn" @mouseenter="onYesHover">Yes</button>
      <button
        v-if="showNo"
        class="no-btn"
        :style="noStyle"
        @mouseenter="moveNoButton"
      >
        No
      </button>
    </div>
    <p v-if="showNote" class="note">Please click on yes 🐻❤️</p>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import bear from "./assets/bear.png"; // or .jpg depending on your file

export default defineComponent({
  name: "App",
  setup() {
    const noStyle = ref({
      left: "60%",
      top: "0%"
    });

    const showNo = ref(true);
    const showNote = ref(false);

    const moveNoButton = () => {
      const x = Math.random() * 70;
      const y = Math.random() * 60;

      noStyle.value = {
        left: `${x}%`,
        top: `${y}%`
      };
    };

    const onYesHover = () => {
      showNo.value = false;
      showNote.value = true;
    };

    return {
      bear,
      noStyle,
      showNo,
      showNote,
      moveNoButton,
      onYesHover
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
  margin-top: 25px;
  position: relative;
  width: 300px;
  height: 120px;
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
  position: absolute;
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
</style>
