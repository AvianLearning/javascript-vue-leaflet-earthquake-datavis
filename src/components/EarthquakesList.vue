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
        <tr v-for="(earthquake, index) in earthquakes" :earthquake="earthquake" :key="index">
            <td v-on:click="handleClick">{{ earthquake.properties.place }}</td>
            <td>{{ earthquake.properties.mag }}</td>
        </tr>
    </tbody>
</table>
</div>
  
</template>

<script>
import { eventBus } from '../main.js';

export default {
    name: 'earthquakes-list',
    props: ['earthquakes'],
    methods: {
        handleClick(){
            eventBus.$emit('earthquake-selected', this.earthquake);
        }
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