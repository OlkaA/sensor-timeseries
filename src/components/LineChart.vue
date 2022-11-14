<template>
  <div :id="id">
    <apexchart type="line" height="350" :options="options" :series="series"></apexchart>
  </div>
</template>

<script>
import VueApexCharts from 'vue-apexcharts'

export default {
  name: 'LineChart',
  components: {
    apexchart: VueApexCharts,
  },
  props: {
    options: {
      type: Object
    },
    series: {
      type: Array
    },
    id: {
      type: String
    },
  }
}
</script>
  