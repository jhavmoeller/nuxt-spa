<template>
  <main>
    Default: {{ pageContent }}
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
}
</script>

<style>
main {
  border: 1px solid red;
}
</style>
