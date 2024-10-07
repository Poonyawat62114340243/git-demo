<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" class="logo">
    <button @click="incrementCount" class="click-button">
      เพิ่มข้อมูล: {{ count }}
    </button>
    <div class="chart-container">
      <canvas id="myChart"></canvas>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'
import { Chart } from 'chart.js/auto'

export default {
  setup() {
    const count = ref(0)
    let myChart = null;

    const incrementCount = () => {
      count.value++
      updateChart()
    }

    const updateChart = () => {
      if (myChart) {
        myChart.data.datasets[0].data.push(count.value)
        myChart.data.labels.push(`ครั้งที่ ${count.value}`)
        myChart.update()
      }
    }

    onMounted(() => {
      const ctx = document.getElementById('myChart').getContext('2d');
      myChart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: [],
          datasets: [{
            label: 'จำนวนการคลิก',
            data: [],
            borderColor: 'rgba(75, 192, 192, 1)',
            borderWidth: 2,
            fill: true,
            backgroundColor: 'rgba(75, 192, 192, 0.2)',
          }]
        },
        options: {
          scales: {
            y: {
              beginAtZero: true
            }
          }
        }
      });
    })

    return {
      count,
      incrementCount
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.logo {
  max-width: 150px;
  margin-bottom: 20px;
}

.click-button {
  background-color: #42b983;
  border: none;
  padding: 10px 20px;
  color: white;
  font-size: 16px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.click-button:hover {
  background-color: #38a375;
}

.chart-container {
  max-width: 600px;
  margin: 40px auto;
  padding: 20px;
  border: 2px solid #2c3e50;
  border-radius: 10px;
  background-color: #f5f5f5;
}

canvas {
  max-width: 100%;
  height: auto;
}
</style>
