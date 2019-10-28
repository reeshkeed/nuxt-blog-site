<template>
  <div class="bg-indigo-800 h-screen">
    <div class="container mx-auto">
      <div class="v-centered w-full h-screen mx-auto max-w-md">
        <div class="w-full">
          <form @submit.prevent="registerUser" class="bg-white shadow-md rounded px-10 py-12 mb-4">
            <p class="font-black text-2xl mb-4 text-indigo-700 text-center uppercase">Register</p>

            <Notification :message="error" v-if="error"/>

            <div class="mb-4">
              <label class="block text-gray-700 text-sm font-bold mb-2" for="name">
                Name
              </label>
              <input v-model="name"  class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" type="text" placeholder="Name" required autofocus>
            </div>
            <div class="mb-4">
              <label class="block text-gray-700 text-sm font-bold mb-2" for="username">
                Username
              </label>
              <input v-model="username"  class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" type="text" placeholder="Username" required>
            </div>
            <div class="mb-4">
              <label class="block text-gray-700 text-sm font-bold mb-2" for="email">
                Email
              </label>
              <input v-model="email"  class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" type="email" placeholder="Email" required>
            </div>
            <div class="mb-6">
              <label class="block text-gray-700 text-sm font-bold mb-2" for="password">
                Password
              </label>
              <input v-model="password" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline" id="password" type="password" placeholder="Password" required>
            </div>
            <div class="flex items-center justify-between">
              <button class="bg-indigo-500 hover:bg-indigo-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="submit">
                Register
              </button>
              <nuxt-link :to="{ name: 'login' }" class="inline-block align-baseline font-bold text-sm text-indigo-500 hover:text-indigo-700">
                Login
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
import Notification from '~/components/Notification';
import { mapGetters } from 'vuex';

export default {
  middleware: 'guest',
  
  components: {
    Notification,
  },

  data() {
    return {
        name: '',
        username: '',
        email: '',
        password: '',
        error: null
     }
   },

   methods: {
     async registerUser() {
       try {
         await this.$axios.post('register', {
           name: this.name,
           username: this.username,
           email: this.email,
           password: this.password
         })

         await this.$auth.login({
           data: {
             username: this.username,
             password: this.password
           }
         })

         this.$router.push('/');
       } catch (e) {
         this.error = e.response.data;
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
