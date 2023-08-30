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
            <button class="ui button search-button">Search</button>
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
            <th>
              <button class="ui button" @click="deleteSelected">Delete Selected</button>
            </th>
            <th>Searched Places</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="place in paginatedSearchedPlaces" :key="place.id">
            <td><input type="checkbox" v-model="place.selected"></td>
            <td>{{ place.name }}</td>
          </tr>
        </tbody>
      </table>
      <div class="pagination-controls">
        <button class="ui button" @click="prevPage" :disabled="currentPage <= 1">Previous</button>
        <span>Page {{ currentPage }} of {{ totalPages }}</span>
        <button class="ui button" @click="nextPage" :disabled="currentPage >= totalPages">Next</button>
      </div>

    </section>

    <!-- Time -->
    <div v-if="timeZone && localTime" class="localtime">
      <p>Last Search</p>
      <p>Time Zone: {{ timeZone }}</p>
      <p>Local Time: {{ localTime }}</p>
    </div>

  </div>
</template>

<script>
/* eslint-disable no-undef, no-unused-vars */
import axios from 'axios';
import { toRaw } from 'vue';
const apiKey = process.env.VUE_APP_GOOGLE_MAPS_API_KEY;

export default {

  data() {
    return {
      location: null,
      searchLocation: null,
      markers: [],
      map: null,
      searchedPlaces: [],
      selectedPlaces: [],
      currentPage: 1,
      itemsPerPage: 10,
      timeZone: null,
      localTime: null
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

  computed: {
    paginatedSearchedPlaces() {
      const start = (this.currentPage - 1) * this.itemsPerPage;
      const end = start + this.itemsPerPage;
      return this.searchedPlaces.slice(start, end);
    },
    totalPages() {
      return Math.ceil(this.searchedPlaces.length / this.itemsPerPage);
    }
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

    showCurrentLocationOnMap(lat, lng, id) {

      this.map.panTo(new google.maps.LatLng(lat, lng));

      const newMarker = new google.maps.Marker({
        position: new google.maps.LatLng(lat, lng),
        map: this.map,
        id: id
      });
      console.log("new marker id", newMarker.id);
      this.markers.push(newMarker);
    },

    handleSearch() {
      if (this.searchLocation && this.searchLocation.geometry) {
        const lat = this.searchLocation.geometry.location.lat();
        const lng = this.searchLocation.geometry.location.lng();
        const id = Date.now();
        this.showCurrentLocationOnMap(lat, lng, id);
        console.log("newId is", id);
        // Save to searchedPlaces
        this.searchedPlaces.push({
          id: id,
          name: this.searchLocation.name,
          lat: lat,
          lng: lng
        });

        //console.log(this.searchedPlaces);

        this.fetchTimeZone(lat, lng);
      }
    },

    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },

    deleteSelected() {
      const selectedIds = this.searchedPlaces.filter(place => place.selected).map(place => place.id);

      // Filter out markers that are not in the list of selected IDs and remove them from the map
      this.markers = this.markers.filter(marker => {
        if (selectedIds.includes(toRaw(marker).id)) {
          toRaw(marker).setMap(null);
          return false;
        }
        return true;
      });

      // Now, filter out the selected places from the searchedPlaces array
      this.searchedPlaces = this.searchedPlaces.filter(place => !place.selected);
    },

    fetchTimeZone(lat, lng) {
      const timestamp = Math.floor(Date.now() / 1000);
      axios.get(`https://maps.googleapis.com/maps/api/timezone/json?location=${lat},${lng}&timestamp=${timestamp}&key=${apiKey}`)
        .then(response => {
          this.timeZone = response.data.timeZoneId;
          this.updateLocalTime(response.data.rawOffset + response.data.dstOffset);  // raw offset + dst offset
        })
        .catch(error => {
          console.log("Error fetching timezone:", error);
        });
    },



    updateLocalTime(offsetInSeconds) {
      const date = new Date();
      const utcTime = date.getTime() + date.getTimezoneOffset() * 60000; // Convert local time to UTC in milliseconds
      const localTimeInMilliseconds = utcTime + (offsetInSeconds * 1000);
      this.localTime = new Date(localTimeInMilliseconds).toLocaleTimeString();
    }


  }
}

</script>
<style scoped>
.search-button {
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

.localtime {
  position: absolute;
  top: 0;
  right: 0;
  z-index: 1;
  border: solid 1px black;
  background-color: white;
  padding: 5px;
}
</style>


