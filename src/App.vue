<template>
<main>
  <article>
    <hgroup>
    <h1>Earthquakes: data from US Geological Survey</h1>
    <h2>Magnitude 2.5+ earthquakes for the past day</h2>
    <h3>Click on a circle for more details</h3>
    </hgroup>
    <div class="center" id="eq-map"></div>
  </article>   
</main>
</template>

<script>
// import { eventBus } from './main.js';
import "leaflet/dist/leaflet.css";
import L from 'leaflet';

export default {
  name: 'app',
data() {
  return {
   center: [17,-3],
   earthquakes: [],
   myMap: null
  }
},
mounted() {
  this.setupLeafletMap();
  this.fetchData();
},

methods: {
  setupLeafletMap: function () {
    this.myMap = L.map('eq-map', {
    minZoom: 2,
    maxZoom: 18
}).setView(this.center, 2);

  const attribution = '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors';
  const tileUrl = 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png';
  const tiles = L.tileLayer(tileUrl, {attribution});
  tiles.addTo(this.myMap);
 },

fetchData: async function () {
    await fetch("https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/2.5_day.geojson")
    .then(response => response.json())
    .then(earthquakeData => this.earthquakes = earthquakeData.features);

    L.geoJson(this.earthquakes, {
        onEachFeature: function (earthquake, layer) {
          var props = earthquake.properties;

          layer.bindPopup('<a href="' + props.url + '">' + props.title + '</a>');
        },
        pointToLayer: function (earthquake, latlng) {
          var color,
              mag,
              radius;

          mag = earthquake.properties.mag;
          if (mag === null) {
            color = '#fff';
            radius = 2;
          } else {
            color = '#00846b';
            radius = 4 * Math.max(mag, 1);
          }

          return L.circleMarker(latlng, {
            color: color,
            radius: radius
          });
        }
      }).addTo(this.myMap);

    console.log(this.earthquakes);
  },
}
}

</script>

<style>

#eq-map { 
  height: 80vh; 
  width: 80vw;
}

.center {
  margin:auto;
  padding: 10px;
}

hgroup {
  text-align: center;
  font-family: 'Krona One', sans-serif;
}

h2 {
  font-style: italic;
  color: #2F4F4F;
}

h3 {
  color: #808080;
}
</style>
