<template>
  <div id="app">
    <NovaPoshta/>
  </div>
</template>

<script>
import NovaPoshta from './components/NovaPoshta.vue'

export default {
  name: 'App',
  components: {
    NovaPoshta
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
