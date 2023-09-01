<template>
  <div class="localtime" >
    <p>{{ timeZone || "Fetching time zone..." }}</p>
    <p>{{ localTime || "Fetching local time..." }}</p>
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
