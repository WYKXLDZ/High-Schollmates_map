<template>
  <div id="app">
    <Map msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import Map from './components/Map.vue'

export default {
  name: 'App',
  components: {
    Map
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
  /* margin-top: 60px; */
  position:absolute;
  top:0px;
  left: 0px;
  height:100%;
  width:100%;
}
</style>
