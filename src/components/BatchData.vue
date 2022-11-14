<template>
  <div class="batch-data">
    <div v-if="isSensorDataReady">
      <div v-for="sensor in sensors" :key="sensor.id">
        <LineChart :options="sensor.chartOptions" :series="sensor.series" type="line" height="350" :id="sensor.id">
        </LineChart>
      </div>
    </div>
    <div class="spinner-wrapper" v-if="!isGraphRendered">
      <img src="../assets/images/loading.gif" />
    </div>
  </div>
</template>
  
<script>
import LineChart from './LineChart.vue';
import debounce from 'lodash/debounce'

export default {
  name: 'BatchData',
  props: {
    sensorData: Object
  },
  components: {
    LineChart
  },
  data() {
    return {
      isSensorDataReady: false,
      isGraphRendered: false,
      colors: ['#008ffb', '#00e396', '#feb019', '#ff4560', '#775dd0', '#a1de64'],
      sensors: []
    }
  },
  mounted: debounce(function () {
    this.$nextTick(() => {
      this.drawTimelines();
    })
  }, 250),

  updated() {
    this.isGraphRendered = true;
  },

  methods: {
    drawTimelines() {
      let categories = 0;
      const reduceArrayAmount = 10;

      for (const batch in this.sensorData) {
        for (const sensor in this.sensorData[batch]) {
          if (categories < Number(this.sensorData[batch][sensor].timestamps.length / reduceArrayAmount)) {
            categories = Number((this.sensorData[batch][sensor].timestamps.length / reduceArrayAmount).toFixed(0));
          }
          let sensorDataForChart;

          const currentSensor = this.sensors.find(el => el.id === sensor);
          if (!currentSensor) {
            const data = this.getAverage(this.sensorData[batch][sensor].values, reduceArrayAmount);
            sensorDataForChart = {
              id: sensor,
              series: [{
                name: batch,
                data
              }
              ],
              chartOptions: {
                yaxis: {
                  title: {
                    text: this.sensorData[batch][sensor].unit,
                    rotate: 0
                  },
                },
                markers: {
                  size: 0
                },
                dataLabels: {
                  enabled: false,
                },
                colors: this.colors,
                title: {
                  text: sensor,
                  align: 'left'
                },
                stroke: {
                  width: 1,
                  curve: 'straight',
                },
                chart: {
                  toolbar: {
                    tools: {
                      pan: false,
                      download: false,
                    }
                  },
                  animations: {
                    enabled: false,
                    animateGradually: {
                      enabled: false,
                    },
                    dynamicAnimation: {
                      enabled: false,
                    }
                  },
                  labels: {
                    show: false,
                  },
                  height: 350,
                  type: 'line'
                },
              },
            }
            this.sensors.push(sensorDataForChart);
          }
          else {
            const data = this.getAverage(this.sensorData[batch][sensor].values, reduceArrayAmount);
            if (data.length) {
              currentSensor.series.push({
                name: batch,
                data
              })
            }
          }
        }
      }

      let timeArray = [];
      for (let i = 0; i < categories; i++) {
        timeArray.push(i * reduceArrayAmount);
      }

      this.sensors.forEach(el => {
        el.chartOptions.xaxis = {
          categories: timeArray,
          tickAmount: 20,
          type: 'nymeric',
          tooltip: {
            enabled: false
          },
          title: {
            text: "minutes",
            offsetY: 130,
            offsetX: -50
          }
        }
      })

      this.isSensorDataReady = true;
    },

    getAverage(array, amount) {
      let result = [];
      do {
        const amountItemsInArray = array.slice(0, amount);
        const sum = amountItemsInArray.reduce((a, b) => Number(a) + Number(b), 0);
        const avg = (sum / amountItemsInArray.length).toFixed(2) || 0;
        result.push(avg);
        array = array.slice(amount);
      } while (array.length > 0)

      return result;
    }
  }
}
</script>
  
<style scoped lang="scss">
@import '../assets/scss/variables.scss';
.batch-data {
  padding: 2rem;

  .spinner-wrapper {
    position: fixed;
    top: 0;
    right: 0;
    left: 0;
    bottom: 0;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  img {
    height: 50px;
    width: 50px;
  }
}
</style>
  