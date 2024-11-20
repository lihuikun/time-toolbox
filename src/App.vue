<script setup lang="ts">
import TimeProgress from './components/TimeProgress.vue'
</script>

<template>
  <div class="app">
    <TimeProgress />
  </div>
</template>

<style>
:root {
  color-scheme: light dark;
  
  /* Light theme variables */
  --card-bg: #ffffff;
  --card-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  --text-primary: #333333;
  --text-secondary: #666666;
  --progress-bg: #f0f0f0;
  --progress-start: #4f46e5;
  --progress-end: #818cf8;
  --hover-bg: rgba(0, 0, 0, 0.05);
}

:root.dark-theme {
  --card-bg: #1a1a1a;
  --card-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
  --text-primary: #ffffff;
  --text-secondary: #a0a0a0;
  --progress-bg: #333333;
  --progress-start: #6366f1;
  --progress-end: #818cf8;
  --hover-bg: rgba(255, 255, 255, 0.1);
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
    Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background: #f0f2f5;
  margin: 0;
  transition: background-color 0.3s ease;
}

.app {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
}

:root.dark-theme body {
  background: #000000;
}
</style>