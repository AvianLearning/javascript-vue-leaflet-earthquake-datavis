<template>
<main>
  <article>
    <h1>Earthquakes</h1>
    <div id="eq-map"></div>
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
import L from 'leaflet';

export default {
  name: 'app',
data() {
  return {
    earthquakes: [],
    selectedEarthquake: null,
    mags: [],
    lons: [],
    lats: []
  }
},
mounted() {
  this.fetchData();

  this.extractMagnitudes();

  eventBus.$on('earthquake-selected', (earthquake) => {
    this.selectedEarthquake = earthquake
  });

  const myMap = L.map('eq-map').setView([0, 50], 2);

  const attribution = '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors';
  const tileUrl = 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png';
  const tiles = L.tileLayer(tileUrl, {attribution});
  tiles.addTo(myMap);

  const myLayer = L.geoJSON().addTo(myMap);
},
components: {
  "earthquakes-list": EarthquakesList,
  "earthquake-detail": EarthquakeDetail
},
methods: {
  fetchData: function () {
    fetch("https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/2.5_day.geojson").then(response => response.json())
  .then(earthquakeData => this.earthquakes = earthquakeData.features);
  },
  extractMagnitudes: function () {
    for (const earthquake in this.earthquakes) {
      const mag = this.earthquakes.properties.mag
      this.mags.push(mag)
    }
  }
}

}
</script>

<style>

#eq-map { 
  height: 400px; 
  width: 60vw;
}

</style>
