<template>
  <div class="batch-reports">
    <ReportsTable :sensorData="sensorData"></ReportsTable>
  </div>
</template>
  
<script>
import ReportsTable from './ReportsTable.vue';

export default {
  name: "BatchReports",
  props: {
    sensorData: Object
  },
  components: { ReportsTable }
}
</script>