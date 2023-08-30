<template>
  <section class="ui two column centered grid">
    <div class="column">
      <form class="ui segment large form">
        <div class="ui message red"></div>
        <div class="ui segment">
          <div class="field location-search">
            <input v-model="searchTerm" @keyup.enter="searchLocation" placeholder="Enter location" />
          </div>
          <button class="ui button" @click="searchLocation">Search</button>
        </div>
      </form>
    </div>
  </section>


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
        console.log(location);
        // this.setLocationAndMarker(location.lat, location.lng);
      });
    }
  }

};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.ui.button {
  background-color: green;
  color: white;
}
</style>
