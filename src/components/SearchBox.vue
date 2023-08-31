<template>
  <div class="ui two column centered grid">
    <div class="column">
      <form class="ui segment large form" @submit.prevent="$emit('search')">
        <div class="ui segment">
          <div class="field">
            <input type="text" :value="searchLocation" @input="updateLocation" placeholder="Enter location" ref="autocomplete" />
            <i class="map marker alternate icon location-icon" @click="$emit('getCurrentLocation')"></i>
          </div>
          <button class="ui button search-button">Search</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
/* eslint-disable no-undef, no-unused-vars */

export default {
  props: ['searchLocation'],

  watch: {
  searchLocation(newVal, oldVal) {
    console.log('Old Value:', oldVal);
    console.log('New Value:', newVal);
  }
},



  mounted() {
    let autocomplete = new google.maps.places.Autocomplete(this.$refs["autocomplete"],
      {
        bounds: new google.maps.LatLngBounds(
          new google.maps.LatLng(43.6532, -79.3832)
        )
      });

    autocomplete.addListener("place_changed", () => {

      this.$emit('place-selected', autocomplete.getPlace());

    });
  },

  methods: {
    updateLocation(event) {
      this.$emit('update:location', event.target.value);
    }
  }
}
</script>

<style scoped>
.ui {
  position: relative;
  z-index: 1;
}

.search-button {
  background-color: green;
  color: white;
}
</style>

