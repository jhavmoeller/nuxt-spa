<template>
  <main>
    <h1> {{ pageContent.title.rendered }} </h1>
    <div v-html="pageContent.content.rendered"></div>
  </main>
</template>

<script>

export default {
  props: {
    pageContent: {
      type: Object,
      default: () => {},
    },
  },
  mounted() {

  }
}
</script>

<style>
main {
  border: 1px solid blue;
}
</style>
