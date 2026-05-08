<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue'

const x = ref(window.innerWidth / 2)
const y = ref(window.innerHeight / 2)

const isShake = ref(false)

let timer: number | null = null

const moveDot = () => {
  const padding = 100

  x.value =
    Math.random() * (window.innerWidth - padding * 2) + padding

  y.value =
    Math.random() * (window.innerHeight - padding * 2) + padding

  // 移動完成後觸發晃動
  setTimeout(() => {
    isShake.value = true

    setTimeout(() => {
      isShake.value = false
    }, 400)
  }, 300)
}

const handleClick = () => {
  moveDot()
}

onMounted(() => {
  timer = window.setInterval(() => {
    moveDot()
  }, 1500)
})

onBeforeUnmount(() => {
  if (timer) {
    clearInterval(timer)
  }
})
</script>

<template>
  <div class="playground">

    <div class="dot" :class="{ shake: isShake }" :style="{
      left: x + 'px',
      top: y + 'px',
    }" @click="handleClick"></div>

  </div>
</template>

<style scoped>
.playground {
  position: fixed;
  inset: 0;
  background: #111;
  overflow: hidden;
  cursor: none;
}

.dot {
  position: absolute;

  width: 36px;
  height: 36px;

  border-radius: 999px;

  background: #3730ff;

  transform: translate(-50%, -50%);

  transition:
    left 0.3s linear,
    top 0.3s linear;
}

/* 到定位後晃動 */
.shake {
  animation: shake 0.4s ease;
}

@keyframes shake {
  0% {
    transform: translate(-50%, -50%) scale(1);
  }

  20% {
    transform: translate(-48%, -50%) scale(1.08);
  }

  40% {
    transform: translate(-53%, -50%) scale(0.95);
  }

  60% {
    transform: translate(-47%, -50%) scale(1.05);
  }

  80% {
    transform: translate(-52%, -50%) scale(0.98);
  }

  100% {
    transform: translate(-50%, -50%) scale(1);
  }
}
</style>