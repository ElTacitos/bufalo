<template>
  <div class="home">
    <Line :data="data" :options="options" />
    <span class="text">
      Le cours actuel du bufalo est de:
    </span>
    <span class="value">
      {{ currentValue }} gorgées
    </span>
  </div>
</template>

<script setup lang="ts">
import {
  CategoryScale,
  Chart as ChartJS,
  Legend,
  LinearScale,
  LineElement,
  PointElement,
  Title,
  Tooltip,
} from 'chart.js'
import { Line } from 'vue-chartjs'

const maxValue = 10;
const minValue = 2;

ChartJS.register(CategoryScale, LinearScale, PointElement, LineElement, Title, Tooltip, Legend)

function getLast5minLabels() {
  const labels = [];
  for (let i = 0; i < 5; i++) {
    labels.push(new Date(Date.now() - 60*1000*i).toLocaleTimeString());
  }
  return labels.reverse();
}

function getRandomValue(count: number) {
  const values = [];
  for (let i = 0; i < count; i++) {
    values.push(Math.floor(Math.random() * (maxValue-minValue)) + minValue);
  }

  return values;
}
const values = getRandomValue(5);
const currentValue = values[values.length - 1];
const data = {
  labels: getLast5minLabels(),
  datasets: [
    {
      label: 'Cours du bufalo',
      backgroundColor: '#f87979',
      data: values,
      fill: false,
      clip: {left: false, top: 50, right: false, bottom: false}
    }
  ]
}

const options = {
  responsive: true,
  scales: {
    y: {
      min: 0,
      max: 12,
      ticks: {
        stepSize: 1
      }
    }
  },
  plugins: {
    legend: {
      display: false
    }
  }
}
</script>

<style>
html {
  background-color: #2C3333;
  color: #CBE4DE;
  font-family: 'Roboto', sans-serif;
}

body {
  margin: 0;
}

.home {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.text {
  margin-top: 5rem;
  font-size: 1rem;
}

.value {
  font-size: 1.5rem;
  font-weight: bold;
}
</style>