<template>
  <img alt="Vue logo" src="./assets/google-map-logo.png">
  <h1>Welcome to GeoMark</h1>
  <CurrentLocation />
  <LocationSearch />
</template>

<script>
import CurrentLocation from './components/CurrentLocation.vue'
import LocationSearch from './components/LocationSearch.vue';

export default {
  name: 'App',
  components: {
    CurrentLocation,
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
