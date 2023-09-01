<template>
  <section class="pagination">
    <table class="ui celled table">
      <thead>
        <tr>
          <th>
            <i class="trash alternate icon" @click="deleteSelected"></i>
          </th>
          <th>
            <i class="history icon"></i>
          </th>
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
      <button class="ui button" @click="prevPage" :disabled="currentPage <= 1">
        <i class="angle left icon" @click="prevPage" :disabled="currentPage <= 1"></i>
      </button>
      <span>Page {{ currentPage }} of {{ totalPages }}</span>
      <button class="ui button" @click="nextPage" :disabled="currentPage >= totalPages">
        <i class="angle right icon"></i>
      </button>
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
  top: 100px;
  right: 0;
  z-index: 1;
}
</style>
