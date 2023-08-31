<template>
  <div class="localtime" v-if="timeZone && localTime">
    <p>Last Search</p>
    <p>Time Zone: {{ timeZone }}</p>
    <p>Local Time: {{ localTime }}</p>
  </div>
</template>

<script>
export default {
  props: ['timeZone', 'localTime']
}
</script>

<style scoped>

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
