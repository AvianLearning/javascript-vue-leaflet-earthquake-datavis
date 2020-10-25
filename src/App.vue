<template>
<main>
  <h1 class="center">Earthquakes: US Geological Survey data</h1>
  <header class="image">
    <h2 class="center">Magnitude 2.5+ earthquakes for the past day</h2>
  </header>
  <article>
    <h3 class="center">Click on a circle for more details</h3>
    <div class="center" id="eq-map"></div>
  </article>   
</main>
</template>

<script>
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
          const props = earthquake.properties;

          layer.bindPopup('<a href="' + props.url + '">' + props.title + '</a>');
        },
        pointToLayer: function (earthquake, latlng) {
          let color,
              mag,
              radius;

          mag = earthquake.properties.mag;

          return L.circleMarker(latlng, {
            color: '#00846b',
            radius: 6 * mag
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

body {
  font-family: 'Krona One', sans-serif;
  background-color: #2F4F4F;
}

.center {
  margin:auto;
  padding: 10px;
}

.image {
  background-image: url('./assets/seismometer.jpg');
  background-position: center;
  width: 100vw;
  height: auto;
  margin-bottom: 20px;
  margin-top: 20px;
}

hgroup {
  text-align: center;
  margin: 20px;
  
}

h1 {
  color:white;
  text-align: center;
  text-shadow: 2px 2px 0 #7A7A7A;
}
h2 {
  font-style: italic;
  color: white;
  text-align: center;
  text-shadow: 4px 3px 0 #2F4F4F;
  width: 50vw;
  background: rgba(0, 0, 0, 0.4);
  padding: 20px;
}

h3 {
  color: #C4C4C4;
  width: 50vw;
  text-align: center;
}
</style>
