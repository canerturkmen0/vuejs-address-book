<template>
  <b-container fluid class="p-0 vh-100 d-flex flex-column">
    <Navbar />
    <Nuxt />
  </b-container>
</template>

<script>
import Navbar from '../components/Navbar';

export default {
  components: {
    Navbar
  }
}
</script>

<style>
.breakLongTxt {
  /* break the long text to prevent breaking of responsive css */
  table-layout: fixed;
  word-wrap: break-word;
}
</style>