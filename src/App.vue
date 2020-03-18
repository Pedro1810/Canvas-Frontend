<template>
  <div id="app">
    <canvasApp/>
  </div>
</template>

<script>
import canvasApp from './components/canvasApp.vue'

export default {
  name: 'App',
  components: {
    canvasApp
  }
}
</script>

<style>
#app,body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0;
  padding: 0;
  background: #e4e4e4;
}
*{
  box-sizing: border-box;
}
</style>
