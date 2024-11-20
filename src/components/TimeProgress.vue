<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';
import dayjs from 'dayjs';
import dayOfYear from 'dayjs/plugin/dayOfYear';
import isLeapYear from 'dayjs/plugin/isLeapYear';
import ProgressBar from './ProgressBar.vue';

dayjs.extend(dayOfYear);
dayjs.extend(isLeapYear);

const now = ref(dayjs());
const isDark = ref(window.matchMedia('(prefers-color-scheme: dark)').matches);

let timer: number;

const updateTime = () => {
  now.value = dayjs();
};

const toggleTheme = () => {
  isDark.value = !isDark.value;
  document.documentElement.classList.toggle('dark-theme', isDark.value);
};

onMounted(() => {
  timer = setInterval(updateTime, 1000);
  document.documentElement.classList.toggle('dark-theme', isDark.value);
});

onUnmounted(() => {
  clearInterval(timer);
});

const getDayProgress = () => {
  const currentHour = now.value.hour();
  const currentMinute = now.value.minute();
  const totalMinutesInDay = 24 * 60;
  const currentMinutes = currentHour * 60 + currentMinute;
  return Math.round((currentMinutes / totalMinutesInDay) * 100);
};

const getWeekProgress = () => {
  const dayOfWeek = now.value.day();
  return Math.round((dayOfWeek / 7) * 100);
};

const getMonthProgress = () => {
  const currentDate = now.value.date();
  const totalDays = now.value.daysInMonth();
  return Math.round((currentDate / totalDays) * 100);
};

const getYearProgress = () => {
  const dayOfYear = now.value.dayOfYear();
  const totalDays = now.value.isLeapYear() ? 366 : 365;
  return Math.round((dayOfYear / totalDays) * 100);
};
</script>

<template>
  <div class="time-progress">
    <div class="header">
      <h1 class="title">时间统计</h1>
      <button class="theme-toggle" @click="toggleTheme">
        <span v-if="isDark">🌞</span>
        <span v-else>🌙</span>
      </button>
    </div>
    <div class="date">
      今天是 {{ now.format('YYYY 年 MM 月 DD 日') }}
      <span class="time">{{ now.format('HH:mm:ss') }}</span>
    </div>

    <div class="progress-item">
      <div class="text">
        {{ now.format('MM 月 DD 日') }} 过去了 {{ now.format('HH') }} 小时，<br>
        {{ now.format('MM 月 DD 日') }} 还剩余 {{ 24 - now.hour() }} 小时
      </div>
      <ProgressBar :percentage="getDayProgress()" />
      <div class="percentage">已经过去 {{ getDayProgress() }}%</div>
    </div>

    <div class="progress-item">
      <div class="text">
        本周过去了 {{ now.day() }} 天，<br>
        本周还剩余 {{ 7 - now.day() }} 天
      </div>
      <ProgressBar :percentage="getWeekProgress()" />
      <div class="percentage">已经过去 {{ getWeekProgress() }}%</div>
    </div>

    <div class="progress-item">
      <div class="text">
        {{ now.format('MM') }} 月过去了 {{ now.date() }} 天，<br>
        {{ now.format('MM') }} 月还剩余 {{ now.daysInMonth() - now.date() }} 天
      </div>
      <ProgressBar :percentage="getMonthProgress()" />
      <div class="percentage">已经过去 {{ getMonthProgress() }}%</div>
    </div>

    <div class="progress-item">
      <div class="text">
        {{ now.year() }}年过去了 {{ now.dayOfYear() }} 天，<br>
        {{ now.year() }}年还剩余 {{ (now.isLeapYear() ? 366 : 365) - now.dayOfYear() }} 天
      </div>
      <ProgressBar :percentage="getYearProgress()" />
      <div class="percentage">已经过去 {{ getYearProgress() }}%</div>
    </div>
  </div>
</template>

<style scoped>
.time-progress {
  background: var(--card-bg);
  padding: 20px;
  border-radius: 8px;
  width: 100%;
  max-width: 400px;
  box-shadow: var(--card-shadow);
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.title {
  font-size: 1.2em;
  color: var(--text-primary);
  font-weight: 600;
  margin: 0;
}

.date {
  font-size: 0.9em;
  color: var(--text-secondary);
  margin-bottom: 20px;
}

.time {
  display: inline-block;
  color: var(--text-primary);
  font-weight: 500;
  margin-left: 8px;
  min-width: 80px;
}

.theme-toggle {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 1.2em;
  padding: 4px 8px;
  border-radius: 4px;
  transition: background-color 0.3s;
}

.theme-toggle:hover {
  background: var(--hover-bg);
}

.progress-item {
  margin-bottom: 24px;
}

.text {
  font-size: 0.9em;
  color: var(--text-primary);
  margin-bottom: 8px;
  line-height: 1.5;
}

.percentage {
  font-size: 0.9em;
  color: var(--text-secondary);
  margin-top: 4px;
}
</style>