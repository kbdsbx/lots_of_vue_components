<template>
  <div id="app">
    <amap-test></amap-test>
  </div>
</template>

<script>
import AmapTest from './tests/amap.test.vue'

export default {
  name: 'app',
  components: {
    AmapTest,
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  position: absolute;
  width: 100%;
  height: 100%;
  left: 0;
  top: 0;
}
</style>
