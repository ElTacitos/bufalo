<template>
  <div class="home" v-if="loaded" >
    <Line :data="chartData" :options="chartOptions"/>
    <span class="text">
      Le cours actuel du bufalo est de:
    </span>
    <span class="value">
      {{ currentValue }} gorgées
    </span>
  </div>
</template>

<script>
import { Line } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, LineElement, PointElement, CategoryScale, LinearScale } from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, LineElement, PointElement, CategoryScale, LinearScale)

export default {
  name: 'BarChart',
  components: { Line },
  data: () => ({
    loaded: false,
    chartOptions: {
      responsive: true,
      scales: {
        y: {
          min: 0,
          max: 10,
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
    },
    chartData: {
      labels: ['Janvier', 'Février', 'Mars', 'Avril', 'Mai', 'Juin', 'Juillet', 'Août', 'Septembre', 'Octobre', 'Novembre', 'Décembre'],
      datasets: [
        {
          label: 'Cours du bufalo',
          backgroundColor: '#f87979',
          data: [40, 39, 10, 40, 39, 80, 40, 39, 10, 40, 39, 80],
          fill: false,
          clip: {left: false, top: 50, right: false, bottom: false}
        }
      ]
    },
    currentValue: 0
  }),
  async mounted () {
    this.loaded = false

    try {
      const response = await fetch('https://arxkqpaohfbusj4uiz4ja6bsru0eqwpl.lambda-url.eu-west-3.on.aws/')
      const data = await response.json()

      data.sort((a, b) => a.id - b.id)

      this.chartData.datasets[0].data = data.map((item) => item.value)
      this.chartData.labels = data.map((item) => new Date(Number(item.id)).toLocaleTimeString())

      this.currentValue = data[data.length - 1].value

      this.loaded = true
    } catch (e) {
      console.error(e)
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