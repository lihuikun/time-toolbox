<script setup lang="ts">
defineProps<{
  percentage: number;
}>();
</script>

<template>
  <div class="progress-bar">
    <div 
      class="progress-fill"
      :style="{ width: `${percentage}%` }"
    ></div>
  </div>
</template>

<style scoped>
.progress-bar {
  height: 12px;
  background: var(--progress-bg);
  border-radius: 6px;
  overflow: hidden;
  position: relative;
}

.progress-fill {
  height: 100%;
  background: linear-gradient(90deg, 
    var(--progress-start) 0%, 
    var(--progress-end) 100%
  );
  border-radius: 6px;
  position: relative;
  transition: width 0.5s ease-in-out;
}

.progress-fill::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(
    90deg,
    rgba(255, 255, 255, 0) 0%,
    rgba(255, 255, 255, 0.1) 50%,
    rgba(255, 255, 255, 0) 100%
  );
  animation: shimmer 2s infinite;
}

@keyframes shimmer {
  0% {
    transform: translateX(-100%);
  }
  100% {
    transform: translateX(100%);
  }
}
</style>