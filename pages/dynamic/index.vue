<template>
  <component :is="component" :page-content="page" />
</template>

<script>
import axios from 'axios';

// Dynamic components
const components = {
  pDefault: () => import('@/components/page-layouts/TheDefaultPage.vue' /* webpackChunkName: "components/page-layouts/TheDefaultPage" */),
  pContent: () => import('@/components/page-layouts/TheContentPage.vue' /* webpackChunkName: "components/page-layouts/TheContentPage" */),
  pPost: () => import('@/components/page-layouts/ThePostPage.vue' /* webpackChunkName: "components/page-layouts/ThePostPage" */),
}

export default {
  components,
  head() {
    return {
      title: `${this.page.name} | AWESOME`,
      meta: [
        { hid: 'description', name: 'description', content: `My custom description for ${this.page.name}` },
      ],
    };
  },
  // validate ({ params }) {
  //   // Must be a number
  //   return /^\d+$/.test(params.url)
  // },
  async asyncData({ params, error }) {
    console.log(params);  
    const slug = params[0].replace(/^\/+/g, ''); // Remove leading slash
    try {
      var { data } = await axios.get(`https://wpdev.jonashavmoeller.dk/wp-json/wp/v2/posts?slug=${slug}`);
      if (!data.length) {
        // Dirty look-up to check if the post is a page
        var { data } = await axios.get(`https://wpdev.jonashavmoeller.dk/wp-json/wp/v2/pages?slug=${slug}`);
        if (!data.length) {
          throw '';
        }
      }
      return {
        component: data[0].type === 'page' ? 'pContent' : 'pPost',
        page: data[0]
      };
    } catch (e) {
      error({ statusCode: 500, message: 'Produktet blev ikke fundet' });
    }
  },
}
</script>
