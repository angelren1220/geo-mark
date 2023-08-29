<template>
  <div class="current-location">
    <button class="ui button"  @click="getCurrentLocation"><i class="map marker alternate icon location-icon"></i></button>
    <!-- Displaying the latitude and longitude -->
    <div v-if="location">
      {{ location }}
    </div>
  </div>
</template>

<script>
import axios from 'axios';
const apiKey = process.env.VUE_APP_GOOGLE_MAPS_API_KEY;

export default {

  data() {
    return {
      location: null
    };
  },

  methods: {

    getCurrentLocation() {

      if(navigator.geolocation){
        navigator.geolocation.getCurrentPosition(position => {
          const lat = position.coords.latitude;
          const lng = position.coords.longitude;
          console.log(lat, lng);
          this.getAddressFrom(lat, lng);
        });
      } else {
        console.log("Browser dose not support Geolocation API");
      }
    },

    getAddressFrom(lat, lng) {
      axios.get("https://maps.googleapis.com/maps/api/geocode/json?latlng=" + lat + "," + lng + `&key=${apiKey}`)
        .then(response => {
          if (response.data.error_message) {
            console.log(response.data.error_message);
          } else {
            this.location = response.data.results[0].formatted_address
            console.log(this.location);
          }
        })
        .catch(error => {
          console.log(error.message);
        });
    }

  }
}

</script>
<style scoped>
.location-icon:hover {
  color: red;
}
</style>


