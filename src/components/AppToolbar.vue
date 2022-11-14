<template>
  <div class="toolbar">
    {{ title }}
  </div>
</template>

<script>
export default {
  name: 'AppToolbar',
  props: {
    title: String
  }
}
</script>

<style scoped lang="scss">
@import '../assets/scss/variables.scss';

.toolbar {
  position: sticky;
  top: 0;
  right: 0;
  left: 0;
  background-color: $green;
  color: white;
  padding: 10px;
  display: flex;
  justify-content: center;
  text-transform: uppercase;
  font-weight: bold;
  letter-spacing: 4px;
  z-index: 30;
}
</style>
