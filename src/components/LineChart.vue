<template>
  <div :id="id">
    <apexchart type="line" height="350" :options="options" :series="series"></apexchart>
  </div>
</template>

<script>
// import Vue from 'vue'
import VueApexCharts from 'vue-apexcharts'
// Vue.use(VueApexCharts);

export default {
  name: 'LineChart',
  components: {
    apexchart: VueApexCharts,
  },
  props: {
    options: {
      type: Object
    },
    height: {
      type: String
    },
    type: {
      type: String
    },
    series: {
      type: Array
    },
    id: {
      type: String
    },
  },
  data() {
    return {
    }
  },
  computed: {
  },
  mounted() {
    console.log('mounted')
  }
}
</script>

<style scoped lang="scss">
.line-wrapper {
  width: 100%;

  .line-chart {
    margin-top: 20px;
  }
}
</style>
  