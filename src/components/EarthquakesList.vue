<template lang="html">
<div>
    <table>
    <thead>
        <tr>
            <th colspan="2">Earthquakes</th>
        </tr>
        <tr>
      <th>Place</th>
      <th>Magnitude</th>
    </tr>
    </thead>
    <tbody>
        <table-row v-for="(earthquake, index) in earthquakes" :earthquake="earthquake" :key="index">
        </table-row>
    </tbody>
</table>
</div>
  
</template>

<script>
import { eventBus } from '../main.js';
import EarthquakeItem from './EarthquakeItem.vue';

export default {
    name: 'earthquakes-list',
    props: ['earthquakes'],
    components: {
        "table-row": EarthquakeItem
    }
}
</script>

<style lang="css" scoped>

table {
    height: 50vh;
    overflow-y: auto;
    cursor: pointer;
}


</style>