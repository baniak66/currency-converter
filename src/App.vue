<template>
  <div id="app">
    <img src="./assets/logo.png">
    <Converter></Converter>
  </div>
</template>

<script>
import Converter from './components/converter'

export default {
  name: 'app',
  components: {
    Converter
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
