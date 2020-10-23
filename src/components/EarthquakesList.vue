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
            <td>{{ earthquake.properties.place }}</td>
            <td>{{ earthquake.properties.mag }}</td>
        </tr>
    </tbody>
</table>
</div>
  
</template>

<script>
export default {
    name: 'earthquakes-list',
    props: ['earthquakes']

}
</script>

<style lang="css" scoped>

</style>