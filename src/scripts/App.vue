<template>
  <section class="wrapper">
    <canvas id="canvas" />

    <section class="button-area">
      <button @click="back">&lt;</button>
      <button @click="go">&gt;</button>
    </section>
  </section>
</template>

<style>
.wrapper {
  width: 90vw;
  height: 80vh;
}

.button-area {
  display: flex;
  justify-content: space-between;
}

button {
  padding: 1em 2.617em;
  background-color: transparent;
  border: 1px solid hsla(200, 15%, 73%, 1);
  border-radius: 2px;
  color: hsla(200, 15%, 73%, 1);
  transition-duration: 0.35s;
  transition-property: background-color, border-color, color;
}

button:hover {
  background-color: hsla(200, 15%, 73%, 0.2);
  border-color: hsla(200, 15%, 73%, 0.7);
  color: hsla(200, 15%, 63%, 1);
}
</style>

<script>
import Chart from 'chart.js'
import moment from 'moment'

export default {
  data () {
    return {
      chart: '',
      chartData: {}
    }
  },
  methods: {
    back () {
      const data = this.chart.data
      const labels = data.labels
      const firstLabel = labels[0]
      labels.unshift(this.calcDate(moment(firstLabel), 1))
      labels.pop()

      const lineData = data.datasets[0].data
      lineData.unshift(this.getLineValue())
      lineData.pop()

      const barData = data.datasets[1].data
      barData.unshift(this.getBarValue())
      barData.pop()

      this.chart.update()
    },
    go () {
      const data = this.chart.data
      const labels = data.labels
      const lastLabel = labels[labels.length - 1]
      labels.push(this.calcDate(moment(lastLabel), 1))
      labels.shift()

      const lineData = data.datasets[0].data
      lineData.push(this.getLineValue())
      lineData.shift()

      const barData = data.datasets[1].data
      barData.push(this.getBarValue())
      barData.shift()

      this.chart.update()
    },
    getLineValue () {
      return Math.round(Math.random() * 10 ** 2) / 10 ** 2
    },
    getBarValue () {
      return Math.floor(Math.random() * 10 ** 6)
    },
    calcDate (day, val) {
      return day.add(val, 'days')
    },
    getData () {
      const today = moment()
      this.chartData = {
        type: 'bar',
        data: {
          labels: [
            today,
            this.calcDate(today, 1),
            this.calcDate(today, 1),
            this.calcDate(today, 1),
            this.calcDate(today, 1),
            this.calcDate(today, 1),
            this.calcDate(today, 1)
          ],
          datasets: [{
            type: 'line',
            data: [
              this.getLineValue(),
              this.getLineValue(),
              this.getLineValue(),
              this.getLineValue(),
              this.getLineValue(),
              this.getLineValue(),
              this.getLineValue()
            ],
            yAxisID: 'y-axis-2',
            fill: false,
            pointBackgroundColor: [
              'hsla(0, 89%, 47%, 1)',
              'hsla(26, 89%, 47%, 1)',
              'hsla(52, 89%, 47%, 1)',
              'hsla(78, 89%, 47%, 1)',
              'hsla(130, 89%, 47%, 1)',
              'hsla(182, 89%, 47%, 1)',
              'hsla(234, 89%, 47%, 1)'
            ]
          },
          {
            data: [
              this.getBarValue(),
              this.getBarValue(),
              this.getBarValue(),
              this.getBarValue(),
              this.getBarValue(),
              this.getBarValue(),
              this.getBarValue()
            ],
            yAxisID: 'y-axis-1',
            backgroundColor: [
              'hsla(0, 89%, 47%, 0.2)',
              'hsla(26, 89%, 47%, 0.2)',
              'hsla(52, 89%, 47%, 0.2)',
              'hsla(78, 89%, 47%, 0.2)',
              'hsla(130, 89%, 47%, 0.2)',
              'hsla(182, 89%, 47%, 0.2)',
              'hsla(234, 89%, 47%, 0.2)'
            ]
          }]
        },
        options: {
          legend: {
            display: false
          },
          scales: {
            yAxes: [
              {
                id: 'y-axis-1',
                position: 'left',
                gridLines: {
                  display: false
                },
                ticks: {
                  beginAtZero: true
                }
              },
              {
                id: 'y-axis-2',
                position: 'right',
                gridLines: {
                  display: false
                },
                ticks: {
                  beginAtZero: true
                }
              }
            ]
          }
        }
      }
    }
  },
  created () {
    // fetch したつもり
    this.getData()
  },
  mounted () {
    this.$nextTick(() => {
      const ctx = this.$el.querySelector('#canvas').getContext('2d')

      this.chart = new Chart(ctx, this.chartData)
    })
  }
}
</script>
