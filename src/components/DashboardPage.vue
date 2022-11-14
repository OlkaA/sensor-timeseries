<template>
  <div>
    <AppToolbar title="Ingredients production" />
    <AppTabs :tabs="tabs" />
    <BatchData v-if="findActiveTab('batchData')" :sensorData="sensorData" />
    <BatchReports v-if="findActiveTab('reports')" :sensorData="sensorData" />
  </div>
</template>

<script>
import AppToolbar from './AppToolbar.vue';
import AppTabs from './AppTabs.vue';
import BatchData from './BatchData.vue';
import BatchReports from './BatchReports.vue';
import json from '../data/data.json';

export default {
  name: 'DashboardPage',
  props: {
    msg: String
  },
  components: {
    AppToolbar,
    AppTabs,
    BatchData,
    BatchReports
  },
  data() {
    return {
      tabs: [
        {
          id: 'batchData',
          title: 'Batch data',
          active: true
        },
        {
          id: 'reports',
          title: 'Reports',
          active: false
        }
      ],
      sensorData: json.data
    }
  },
  methods: {
    findActiveTab(tabId) {
      return this.tabs.find(el => el.id === tabId && el.active)
    }
  }
}
</script>