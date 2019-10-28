<template>
  <div class="container mx-auto mt-24 px-16">
    <div class="bg-white rounded p-16 pl-16">
      <h1 class="font-bold text-4xl mb-2 text-indigo-700">Profile</h1>

      <p class="text-lg mb-2">
        <strong class="text-gray-700">Name :</strong>
        <span class="block sm:inline">{{ loggedInUser.name }}</span>
      </p>

      <p class="text-lg mb-2">
        <strong class="text-gray-700">Username :</strong>
        <span class="block sm:inline">{{ loggedInUser.username }}</span>
      </p>

      <p class="text-lg mb-2">
        <strong class="text-gray-700">Email :</strong>
        <span class="block sm:inline">{{ loggedInUser.email }}</span>
      </p>
    </div>
  </div>

</template>

<script>
import { mapGetters } from 'vuex';

export default {
  computed: {
    ...mapGetters(['loggedInUser'])
  }
}
</script>

<style lang="css" scoped>
</style>
