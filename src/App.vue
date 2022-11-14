<template>
  <div id="app">
    <DashboardPage/>
  </div>
</template>

<script>
import DashboardPage from './components/DashboardPage.vue'

export default {
  name: 'App',
  components: {
    DashboardPage
  }
}
</script>

<style>
html, body {
  margin: 0;
  padding: 0;
  min-height: 100vh;
  font-family: "Calibri"
}
</style>
