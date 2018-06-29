<template>
  <section class="container">
    <div>
      <h1 class="title">
        Test {{ theId }}
      </h1>
      <ul>
        <li v-for="page in content" :key="page.id">
          <nuxt-link :to="'/' + page.slug + '/'">{{ page.title.rendered }}</nuxt-link>
        </li>
      </ul>
    </div>
  </section>
</template>

<script>
import axios from 'axios';

export default {
  computed: {
    theId() {
      return this.$route.params.id;
    },
  },
  async asyncData({ params }) {
    try {
      const { data } = await axios.get(`https://wpdev.jonashavmoeller.dk/wp-json/wp/v2/posts?categories=${params.id}`);
      return { content: data };
    } catch (error) {
      console.log(error);
      return {
        content: {
          name: 'Unknown',
        },
      };
    }
  },
}
</script>

<style>
.container
{
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
.title
{
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}
.subtitle
{
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}
.links
{
  padding-top: 15px;
}
</style>
