<template>
  <div class="current-location">
    <button @click="getCurrentLocation">Get Current Location</button>
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

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.current-location {
  display: flex;
  gap: 10px;
}
</style>



