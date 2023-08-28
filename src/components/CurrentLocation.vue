<template>
  <div class="current-location">
    <i class="map marker alternate icon location-icon" @click="getCurrentLocation"></i>
    <!-- Displaying the latitude and longitude -->
    <div v-if="location">
      Latitude: {{ location.lat }}, Longitude: {{ location.lng }}
    </div>  
  </div>
</template>

<script>
export default {

  data() {
    return {
      location: null
    };
  },

  methods: {
    getCurrentLocation() {
      navigator.geolocation.getCurrentPosition(position => {
        const lat = position.coords.latitude;
        const lng = position.coords.longitude;
        // this.setLocationAndMarker(lat, lng);
        this.location = { lat, lng };
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


