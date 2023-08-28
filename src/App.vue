<template>
  <img alt="Vue logo" src="./assets/google-map-logo.png">
  <LocationSearch msg="Welcome to GeoMark App"/>
</template>

<script>
import LocationSearch from './components/LocationSearch.vue';

export default {
  name: 'App',
  components: {
    LocationSearch
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
