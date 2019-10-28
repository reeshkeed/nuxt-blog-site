<template lang="html">
  <div>
    <nav class="flex items-center justify-between flex-wrap bg-indigo-800 p-5 fixed top-0 left-0 right-0">
      <div class="flex items-center flex-shrink-0 text-white mr-6">
        <svg xmlns="http://www.w3.org/2000/svg" width="30" viewBox="0 0 302.7 302.7">
          <path fill="#FFF" d="M168.7 6.6c18.3 0 32.9 2.8 43.8 8.3 13.1 6.7 23.1 15.8 30.2 27.5 7.1 11.7 10.6 24.7 10.6 39.2 0 13.6-3.6 26.2-10.8 37.7-7.2 11.5-17.1 20.6-29.6 27.3 15 5.3 27.6 14.5 37.9 27.5 10.3 13.1 15.4 27.8 15.4 44.2 0 14.5-3.7 27.7-11 39.8-7.4 12.1-17.2 21.6-29.4 28.6-6.1 3.3-12.9 5.8-20.4 7.5-7.5 1.7-17.5 2.5-30 2.5H46.9V6.6h121.8z"/>
        </svg>
        <span class="font-light text-xl mx-2 tracking-tight">nuxt-blog-site</span>
      </div>
      <div class="w-full lg:flex lg:items-right lg:w-auto">
        <div>
          <template v-if="!isAuthenticated">
            <nuxt-link :to="{ name: 'login' }" class="inline-block text-sm px-5 py-3 leading-none border bg-indigo-400 rounded text-white border-indigo-400 hover:border-transparent hover:text-indigo-400 hover:bg-white mt-4 lg:mt-0">Login</nuxt-link>
            <nuxt-link :to="{ name: 'register' }" class="inline-block text-sm px-5 py-3 leading-none border rounded text-white border-white hover:border-transparent hover:text-indigo-400 hover:bg-white mt-4 lg:mt-0">Register</nuxt-link>
          </template>

          <template v-if="isAuthenticated">
            <div class="flex flex-row">
              <nuxt-link :to="{ name: '/' }" class="font-bold inline-block px-5 py-3 leading-none rounded text-white hover:text-indigo-200 mt-4 lg:mt-0">{{ loggedInUser.name }}</nuxt-link>
              <a  @click.prevent="logout" href="#" class="inline-block text-sm px-5 py-3 leading-none rounded text-indigo-600 hover:text-white mt-4 lg:mt-0">Logout</a>
            </div>
          </template>
        </div>
      </div>
    </nav>
  </div>
</template>

<script>
import { mapGetters } from 'vuex';

export default {
  methods: {
    async logout() {
      await this.$auth.logout();
    },
  },

  computed: {
    ...mapGetters(['isAuthenticated', 'loggedInUser'])
  }
}
</script>

<style lang="css" scoped>
</style>
