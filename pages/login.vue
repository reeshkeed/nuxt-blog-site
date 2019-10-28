<template>
  <div class="bg-indigo-800 h-screen">
    <div class="container mx-auto">
      <div class="v-centered w-full h-screen mx-auto max-w-md">
        <div class="w-full">
          <form @submit.prevent="loginUser" class="bg-white shadow-md rounded px-10 py-16 mb-4">
            <p class="font-black text-2xl mb-4 text-indigo-700 text-center uppercase">Login</p>

            <Notification :message="error" v-if="error"/>

            <div class="mb-4">
              <label class="block text-gray-700 text-sm font-bold mb-2" for="username">
                Username
              </label>
              <input v-model="userForm.username"  class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" type="text" placeholder="Username" required autofocus>
            </div>
            <div class="mb-6">
              <label class="block text-gray-700 text-sm font-bold mb-2" for="password">
                Password
              </label>
              <input v-model="userForm.password" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline" id="password" type="password" placeholder="Password" required>
              <p class="text-red-500 text-xs italic">Please choose a password.</p>
            </div>
            <div class="flex items-center justify-between">
              <button class="bg-indigo-500 hover:bg-indigo-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="submit">
                Login
              </button>
              <nuxt-link :to="{ name: 'register' }" class="inline-block align-baseline font-bold text-sm text-indigo-500 hover:text-indigo-700">
                Register
              </nuxt-link>
            </div>
          </form>
          <p class="text-center text-gray-500 text-xs">
            &copy;2019 nuxt-blog-site. All rights reserved.
          </p>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
import Notification from '~/components/Notification'

export default {
  middleware: 'guest',
  
  components: {
      Notification,
  },

  data() {
    return {
      userForm: {
        username: '',
        password: '',
      },

      error: null

    }
  },

  methods: {
    async loginUser() {
      try {
        await this.$auth.login({
          data: this.userForm
        })

        this.$router.push({
          path: '/'
        });
      } catch (e) {
        this.error = e.response.data
      }
    }
  }
}
</script>

<style scoped>
.v-centered {
  display: flex;
  align-items: center;
}
</style>
