<template>
  <section
  v-if="stationLoading.modalStation"
    class="loading-section-true"
  >
    <h2 class="title" ><img src="/account/gal.svg" alt=""> Операция успешно выполнена</h2>
  </section>
</template>
<script setup>
import { toRefs, ref, defineProps, reactive } from "vue";

const props = defineProps({
  stationLoading: {
    type: Object,
    required: true,
  },
});
const { stationLoading } = toRefs(props);
</script>

<style scoped>

.loading-section-true {
  border: 0.50px solid #79f8c1;
border-radius: 5px;
width: 314px;
height: 60px;
box-shadow: -4px 4px 8px 0 rgba(0, 0, 0, 0.06), 0 0 4px 0 rgba(85, 255, 195, 0.04);
background: #deecd2;
  position: fixed;
  z-index: 100;
  top: 5px;
  right: 15px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.title {
  font-weight: 600;
font-size: 16px;
color: #1b632b;
  display: flex;
  align-items: center;
  gap: 6px;
}

.loading-section-true, .fade-leave-active {
  transition: opacity 0.5s ease;
}
.loading-section-true, .fade-leave-to {
  opacity: 0;
}

.loading-section-true {
  animation: fadeIn 0.5s forwards;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateX(20px); 
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

@media (max-width: 1000px) {
  .loading-section {
    width: 250px;
    height: 25px;
    top: 90px;
    right: 15px;
  }

  .loading-section-true {
    width: 250px;
    height: 25px;
    top: 90px;
    right: 15px;
  }
}

@media (max-width: 550px) {
  .loading-section {
    width: 150px;
    height: 20px;
    top: 90px;
    right: 15px;
  }

  .loading-section-true {
    width: 150px;
    height: 20px;
    top: 90px;
    right: 15px;
  }

  .title {
    font-size: 14px;
    font-weight: 600;
    color: rgb(51, 51, 51);
  }
}
</style>
