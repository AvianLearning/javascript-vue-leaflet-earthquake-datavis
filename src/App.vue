<template>
  <main>
    <h1>Earthquakes</h1>
    <earthquakes-list :earthquakes='earthquakes'></earthquakes-list>
  </main>
</template>

<script>
import EarthquakesList from './components/EarthquakesList.vue';

export default {
  name: 'app',
data() {
  return {
    earthquakes: []
  }
},
mounted() {
  fetch("https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/2.5_day.geojson").then(response => response.json())
  .then(earthquakeData => this.earthquakes = earthquakeData.features);
},
components: {
  "earthquakes-list": EarthquakesList
}

}
</script>

<style>

</style>
