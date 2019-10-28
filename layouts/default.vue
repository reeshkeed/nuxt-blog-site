<template>
  <div class="bg-gray-300">
    <Navbar/>
    <nuxt/>
  </div>
</template>

<script>
import Navbar from './partials/Navbar';
export default {
  components: {
    Navbar
  }
}
</script>

<style>
html, body {
  background: #e2e8f0;
}
</style>
