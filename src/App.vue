<template>
<main>
  <article>
    <h1>Earthquakes</h1>
    <earthquakes-list :earthquakes='earthquakes'></earthquakes-list>
  </article>
  <aside>
    <earthquake-detail :earthquake='selectedEarthquake'></earthquake-detail>
  </aside>
</main>
</template>

<script>
import EarthquakesList from './components/EarthquakesList.vue';
import EarthquakeDetail from './components/EarthquakeDetail.vue';
import { eventBus } from './main.js';

export default {
  name: 'app',
data() {
  return {
    earthquakes: [],
    selectedEarthquake: null
  }
},
mounted() {
  fetch("https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/2.5_day.geojson").then(response => response.json())
  .then(earthquakeData => this.earthquakes = earthquakeData.features);

  eventBus.$on('earthquake-selected', (earthquake) => {
    this.selectedEarthquake = earthquake
  })
},
components: {
  "earthquakes-list": EarthquakesList,
  "earthquake-detail": EarthquakeDetail
}

}
</script>

<style>

</style>
