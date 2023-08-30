<template>
  <!-- <img alt="Vue logo" src="./assets/google-map-logo.png"> -->
  <CurrentLocation />
</template>

<script>
import CurrentLocation from './components/CurrentLocation';

export default {
  name: 'App',
  components: {
    CurrentLocation,
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
