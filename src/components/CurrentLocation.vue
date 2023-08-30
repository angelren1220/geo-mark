<template>
  <div>
    <!-- Search Location -->
    <section class="ui two column centered grid" style="position:relative;z-index:1;">
      <div class="column">
        <form class="ui segment large form" @submit.prevent="handleSearch">
          <!-- Displaying the current address -->
          <div class="ui segment">
            <div class="field">
              <input type="text" v-model="location" placeholder="Enter location" ref="autocomplete" />
              <i class="map marker alternate icon location-icon" @click="getCurrentLocation"></i>
            </div>
            <button class="ui button">Search</button>
          </div>
        </form>
      </div>
    </section>

    <!-- Map -->
    <section id="map">
    </section>

    <!-- Table -->
    <section class="pagination">
      <table class="ui celled table">
        <thead>
          <tr>
            <th>Select</th>
            <th>Searched Places</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="place in searchedPlaces" :key="place.id">
            <td><input type="checkbox" v-model="place.selected"></td>
            <td>{{ place.name }}</td>
          </tr>
        </tbody>
      </table>
    </section>

  </div>
</template>

<script>
/* eslint-disable no-undef, no-unused-vars */
import axios from 'axios';
const apiKey = process.env.VUE_APP_GOOGLE_MAPS_API_KEY;

export default {

  data() {
    return {
      location: null,
      searchLocation: null,
      markers: [],
      map: null,
      searchedPlaces: [],
      selectedPlaces: []
    };
  },

  mounted() {
    this.initMap();

    let autocomplete = new google.maps.places.Autocomplete(this.$refs["autocomplete"],
      {
        bounds: new google.maps.LatLngBounds(
          new google.maps.LatLng(43.6532, -79.3832)
        )
      });

    autocomplete.addListener("place_changed", () => {

      this.searchLocation = autocomplete.getPlace();

    });
  },

  methods: {

    initMap() {
      this.map = new google.maps.Map(document.getElementById("map"), {
        zoom: 15,
        center: new google.maps.LatLng(43.6532, -79.3832),
        mapTypeId: google.maps.MapTypeId.ROADMAP
      });
    },

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
            // console.log(this.location);
          }
        })
        .catch(error => {
          console.log(error.message);
        });
    },

    showCurrentLocationOnMap(lat, lng) {

      this.map.panTo(new google.maps.LatLng(lat, lng));

      const newMarker = new google.maps.Marker({
        position: new google.maps.LatLng(lat, lng),
        map: this.map
      });

      this.markers.push(newMarker);
    },

    handleSearch() {
      if (this.searchLocation && this.searchLocation.geometry) {
        const lat = this.searchLocation.geometry.location.lat();
        const lng = this.searchLocation.geometry.location.lng();
        this.showCurrentLocationOnMap(lat, lng);

        // Save to searchedPlaces
        this.searchedPlaces.push({
          id: Date.now(),
          name: this.searchLocation.name,
          lat: lat,
          lng: lng
        });

        console.log(this.searchedPlaces);
      }
    }


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
}

.pagination {
  position: absolute;
  bottom: 0;
  right: 0;
  z-index: 1;
}
</style>


