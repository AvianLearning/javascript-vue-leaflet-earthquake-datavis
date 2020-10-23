<template lang="html">
<tr>
    <td v-on:click="handleClick">{{ earthquake.properties.place }}</td>
    <td>{{ earthquake.properties.mag }}</td>
</tr>
</template>

<script>
import { eventBus } from '../main.js';

export default {
    name: 'table-row',
    props: ['earthquake'],
    methods: {
        handleClick(){
            eventBus.$emit('earthquake-selected', this.earthquake);
        }
    }
}
</script>

<style lang="css" scoped>

</style>