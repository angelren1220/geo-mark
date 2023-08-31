<template>
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
</template>

<script>
export default {
  props: ['paginatedSearchedPlaces', 'currentPage', 'totalPages'],
  methods: {
    nextPage() {
      this.$emit('next');
    },
    prevPage() {
      this.$emit('prev');
    },
    deleteSelected() {
      this.$emit('delete');
    }
  }
}
</script>

<style scoped>

.pagination {
  position: absolute;
  bottom: 0;
  right: 0;
  z-index: 1;
}

</style>
