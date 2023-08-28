<template>
  <div class="location-search">
    <input v-model="searchTerm" @keyup.enter="searchLocation" placeholder="Enter location" />
    <button @click="searchLocation">Search</button>
  </div>
</template>

<script>
import axios from 'axios';
const apiKey = process.env.VUE_APP_GOOGLE_MAPS_API_KEY;

export default {
  data() {
    return {
      searchTerm: ''
    };
  },
  methods: {
    searchLocation() {
      axios.get(`https://maps.googleapis.com/maps/api/geocode/json?address=${this.searchTerm}&${apiKey}`).then(response => {
        const location = response.data.results[0].geometry.location;
        this.setLocationAndMarker(location.lat, location.lng);
      });
    }
  }

};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
