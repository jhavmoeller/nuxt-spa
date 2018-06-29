<template>
<div>

  <p>Check all these test:</p>
  <no-ssr>
    <ul>
      <li v-for="item in items" :key="item.name">
        {{ item.name }}
      </li>
    </ul>
  </no-ssr>
</div>
</template>
<script>
import axios from 'axios';

export default {
  data() {
    return {
      items: [],
    };
  },
  async mounted() {
    const { data } = await axios.get(`https://swapi.co/api/vehicles/`);
    console.log(data);
    this.items = data.results;
  },
}
</script>

<style>
</style>
