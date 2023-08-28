<template>
  <h1>{{ msg }}</h1>
  <div class="location-search">
    <button @click="getCurrentLocation">Get Current Location</button>
    <input type="text" v-model="searchedLocation" @keyup.enter="searchLocation" placeholder="Search for a location..." />
    <button @click="searchLocation">Search</button>
  </div>
</template>

<script>
export default {
  props: {
    msg: String
  },

  data() {
    return {
      searchedLocation: ''
    };
  },

  methods: {
    async getCurrentLocation() {
      try {
        const position = await new Promise((resolve, reject) => {
          navigator.geolocation.getCurrentPosition(resolve, reject);
        });
        const { latitude, longitude } = position.coords;
        this.$emit('location-selected', { latitude, longitude });
      } catch (error) {
        console.error("Couldn't fetch current location", error);
      }
    },
    searchLocation() {
      if (this.searchedLocation) {
        this.$emit('location-searched', this.searchedLocation);
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/* Add your styles here */
h1 {
  margin: 40px 0 0;
}
.location-search {
  display: flex;
  gap: 10px;
}
</style>



