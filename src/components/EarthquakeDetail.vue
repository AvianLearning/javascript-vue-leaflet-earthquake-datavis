<template lang="html">
  <section v-if='earthquake'>
      <h3>Location: {{ earthquake.properties.place }}</h3>
      <p>Magnitude: {{ earthquake.properties.mag }} </p>
      <p>Co-ordinates: {{ earthquake.geometry.coordinates }}</p>
  </section>
</template>

<script>
import { eventBus } from '../main.js';

export default {
    name: 'earthquake-detail',
    props: ['earthquake']
}

</script>

<style>

</style>