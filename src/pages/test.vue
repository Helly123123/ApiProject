<template>
  <div class="black-fon"></div>
  <div class="action-list">
    <div class="spinner"></div>
  </div>
</template>

<script setup>
import { ref, defineProps } from "vue";

const props = defineProps({
  isLoading: {
    type: Boolean,
    required: true,
  },
});
</script>

<style scoped>
.black-fon {
  position: fixed;
  z-index: 5;
  width: 100%;
  height: 100vh;
  background: rgba(117, 117, 117, 0.3);
  top: 0;
  left: 0;
}

.action-list {
  border-radius: 20px;
  background: #ffffff;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 20;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  gap: 8px;
  padding: 15px;
}

.spinner {
  border: 8px solid rgba(255, 255, 255, 0.3);
  border-top: 8px solid #3498db; /* Цвет спиннера */
  border-radius: 50%;
  width: 50px; /* Ширина спиннера */
  height: 50px; /* Высота спиннера */
  animation: spin 1s linear infinite; /* Анимация вращения */
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
