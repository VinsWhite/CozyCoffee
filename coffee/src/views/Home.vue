<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import Menu from '../components/Menu.vue'

const currentTime = ref(new Date().toLocaleTimeString())

const updateTime = () => {
  currentTime.value = new Date().toLocaleTimeString()
}

const visibleClock = ref(false);

const setVisibleClock = () => {
  visibleClock.value = !visibleClock.value;
}

let timer
onMounted(() => {
  timer = setInterval(updateTime, 1000)
})

onUnmounted(() => {
  clearInterval(timer)
})
</script>

<template>
  <div class="mainTopContainer mx-auto text-center position-relative">
    <div class="topContainer d-flex align-items-center justify-content-center p-5">
      <h3 :class="{'clock-hidden': !visibleClock}" class="text-danger-emphasis mb-3 bg-white border border-2 border-black p-2 rounded-3 w-50 text-center">
        {{ currentTime }}
      </h3>
    </div>
    <Menu />
    <i @click="setVisibleClock" class="bi bi-clock position-absolute top-0 start-0 p-2 text-light"></i> <!-- mostra orologio -->
  </div>
</template>

