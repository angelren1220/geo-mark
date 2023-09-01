<template>
  <div>
    <SearchBox :searchLocationName="searchLocationName" @search="handleSearch" @place-selected="updateSearchLocation"
      @getCurrentLocation="getCurrentLocation" />
    <MapSection @map-initialized="setMapInstance" />
    <PlacesTable :paginatedSearchedPlaces="paginatedSearchedPlaces" :currentPage="currentPage" :totalPages="totalPages"
      @next="nextPage" @prev="prevPage" @delete="deleteSelected" />
    <LocalTimeDisplay :timeZone="timeZone" :localTime="localTime" />
  </div>
</template>
<script>
/* eslint-disable no-undef, no-unused-vars */
import SearchBox from './components/SearchBox.vue';
import MapSection from './components/MapSection.vue';
import PlacesTable from './components/PlacesTable.vue';
import LocalTimeDisplay from './components/LocalTimeDisplay.vue';
import axios from 'axios';
import { toRaw } from 'vue';
const apiKey = process.env.VUE_APP_GOOGLE_MAPS_API_KEY;

export default {
  name: 'App',
  components: {
    SearchBox,
    MapSection,
    PlacesTable,
    LocalTimeDisplay
  },

  data() {
    return {
      localMapInstance: null,
      searchLocation: null,
      searchLocationName: '',
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


    setMapInstance(map) {
      this.localMapInstance = map;
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
            this.searchLocation = response.data.results[0].formatted_address;
            console.log(this.searchLocation);
          }
        })
        .catch(error => {
          console.log(error.message);
        });
    },

    showCurrentLocationOnMap(lat, lng, id) {
      if (this.localMapInstance) {

        console.log("pannnnnn");
        this.localMapInstance.panTo(new google.maps.LatLng(lat, lng));

      }

      const newMarker = new google.maps.Marker({
        position: new google.maps.LatLng(lat, lng),
        map: this.localMapInstance,
        id: id
      });
      console.log("new marker id", newMarker.id);
      this.markers.push(newMarker);
    },

    updateSearchLocation(place) {
      this.searchLocation = place;
      this.searchLocationName = place.formatted_address || place.name;
    },

    handleSearch() {

      console.log("searching...", this.searchLocation);

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

        console.log("searched place is: ", this.searchedPlaces);

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
