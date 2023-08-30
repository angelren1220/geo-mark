<template>
  <!-- Current Location -->
  <div class="current-location">
    <button class="ui button" @click="getCurrentLocation"><i class="map marker alternate icon location-icon"></i></button>
  </div>

  <!-- Search Location -->
  <section class="ui two column centered grid">
    <div class="column">
      <form class="ui segment large form">
        <!-- Displaying the current address -->
        <div v-if="location">
          {{ location }}
        </div>
        <div class="ui segment">
          <div class="field location-search">
            <input v-model="searchTerm" @keyup.enter="searchLocation" placeholder="Enter location" />
          </div>
          <button class="ui button" @click="searchLocation">Search</button>
        </div>
      </form>
    </div>
  </section>

  <!-- Map -->
  <section id="map">

  </section>
</template>

<script>
import axios from 'axios';
const apiKey = process.env.VUE_APP_GOOGLE_MAPS_API_KEY;

export default {

  data() {
    return {
      location: null,
      searchTerm: null
    };
  },

  methods: {

    getCurrentLocation() {

      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(position => {
          const lat = position.coords.latitude;
          const lng = position.coords.longitude;
          console.log(lat, lng);

          this.getAddressFrom(lat, lng);
          this.showCurrentLocationOnMap(lat, lng);
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
            this.location = response.data.results[0].formatted_address;
            console.log(this.location);
          }
        })
        .catch(error => {
          console.log(error.message);
        });
    },

    showCurrentLocationOnMap(lat, lng) {
      // eslint-disable-next-line no-undef, no-unused-vars
      let map = new google.maps.Map(document.getElementById("map"), {
        zoom: 15,
        // eslint-disable-next-line no-undef
        center: new google.maps.LatLng(lat, lng),
        // eslint-disable-next-line no-undef
        mapTypeId: google.maps.MapTypeId.ROADMAP
      });
    },

  }
}

</script>
<style scoped>
.ui.button {
  background-color: green;
  color: white;
}

#map {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background: #136F63;
  z-index: -1;
}
</style>


