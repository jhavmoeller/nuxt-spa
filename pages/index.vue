<template>
  <section class="container">
    <div>
      Indlæg<br>
      <ul>
        <li v-for="page in pages" :key="page.id">
          <nuxt-link :to="'/' + page.slug + '/'">{{ page.title.rendered }}</nuxt-link>
        </li>
      </ul>
    </div>
    <div>
      Kategorier<br>
      <ul>
        <li v-for="category in categories" :key="category.id">
          <nuxt-link :to="'/' + category.slug + '/c-' + category.id + '/'">{{ category.name }}</nuxt-link>
        </li>
      </ul>
    </div>
  </section>
</template>

<script>
import axios from 'axios';
import Logo from '~/components/Logo.vue'
import Vehicles from '~/components/Vehicles.vue'

export default {
  components: {
    Logo,
    Vehicles
  },
  computed: {
    theSlug() {
      return this.$route.params.slug;
    },
  },
  head() {
    return {
      title: `FRONTPAGE| AWESOME`,
      meta: [
        { hid: 'description', name: 'description', content: `Test` },
      ],
    };
  },
  async asyncData({ params, error }) {
    try {
      let [pageRes, categoryRes] = await Promise.all([
        axios.get('https://wpdev.jonashavmoeller.dk/wp-json/wp/v2/posts'),
        axios.get('https://wpdev.jonashavmoeller.dk/wp-json/wp/v2/categories'),
      ]);
      console.log(pageRes);
      return {
        pages: pageRes.data,
        categories: categoryRes.data,
      };
    } catch (e) {
      error({ statusCode: 500, message: 'Produktet blev ikke fundet' });
    }
  },
}
</script>