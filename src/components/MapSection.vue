<template>
  <div id="map"></div>
</template>

<script>
/* eslint-disable no-undef, no-unused-vars */
export default {
  props: ['map'],

  data() {
    return {
      localMapInstance: null  // this will store the Google Map instance
    };
  },

  mounted() {
    this.initMap();
  },

  methods: {
    initMap() {
      this.localMapInstance = new google.maps.Map(document.getElementById("map"), {
        zoom: 15,
        center: new google.maps.LatLng(43.6532, -79.3832),
        mapTypeId: google.maps.MapTypeId.ROADMAP
      });
    }
  }
}
</script>

<style scoped>
#map {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background: #136F63;
}
</style>
