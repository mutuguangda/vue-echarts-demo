<template>
  <div id="app">
    <epidemic-map />
  </div>
</template>

<script>
import EpidemicMap from "./components/EpidemicMap.vue"
export default {
  name: "App",
  components: {
    EpidemicMap
  },
}
</script>

<style lang="scss">
body {
  overflow: hidden;
  margin: 0;
  padding: 0;
}
</style>
