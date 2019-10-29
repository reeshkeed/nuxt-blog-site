<template lang="html">
  <div class="">
    <div class="w-full text-left mt-5">
      <form @submit.prevent="postArticle" class="bg-white border border-indigo-200 rounded px-8 pt-6 pb-8 mb-4">
        <div class="mb-4">
          <label class="block text-gray-700 text-sm font-bold mb-2" for="title">
            Title
          </label>
          <input v-model="title" class="bg-gray-100 appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="title" type="text" placeholder="Title" required>
        </div>
        <div class="mb-6">
          <label class="block text-gray-700 text-sm font-bold mb-2" for="description">
            Description
          </label>
          <textarea v-model="description" class="bg-gray-100 resize-none appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="description" type="text" placeholder="Description" required></textarea>
        </div>
        <div class="flex items-center justify-between">
          <button class="bg-indigo-500 hover:bg-indigo-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="submit">
            POST
          </button>
        </div>
      </form>
    </div>

    <div class="flex flex-col w-full">
      <div
       class="flex flex-row border border-gray-200 bg-white w-full rounded shadow text-left px-4 py-2 my-2"
        v-for="(list, i) in article"
        :key="i"
      >
        <div class="w-full">
          <h1 class="font-bold">{{ list.title }}</h1>
          <p>{{ list.description }}</p>
          <p class="text-gray-600 text-sm">{{ list.created_at | formatDate }}</p>
        </div>

        <div class="flex items-right float-right">
          <button @click="deleteArticle( list.id )" class="hover:text-red-700 text-gray-500 font-bold py-2 px-4 rounded inline-flex items-center">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24"><path class="heroicon-ui fill-current" d="M8 6V4c0-1.1.9-2 2-2h4a2 2 0 0 1 2 2v2h5a1 1 0 0 1 0 2h-1v12a2 2 0 0 1-2 2H6a2 2 0 0 1-2-2V8H3a1 1 0 1 1 0-2h5zM6 8v12h12V8H6zm8-2V4h-4v2h4zm-4 4a1 1 0 0 1 1 1v6a1 1 0 0 1-2 0v-6a1 1 0 0 1 1-1zm4 0a1 1 0 0 1 1 1v6a1 1 0 0 1-2 0v-6a1 1 0 0 1 1-1z"/></svg>
            <span>Delete</span>
          </button>

          <button class="hover:text-green-700 text-gray-500 font-bold py-2 px-4 rounded inline-flex items-center">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24"><path class="heroicon-ui fill-current" d="M6.3 12.3l10-10a1 1 0 0 1 1.4 0l4 4a1 1 0 0 1 0 1.4l-10 10a1 1 0 0 1-.7.3H7a1 1 0 0 1-1-1v-4a1 1 0 0 1 .3-.7zM8 16h2.59l9-9L17 4.41l-9 9V16zm10-2a1 1 0 0 1 2 0v6a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V6c0-1.1.9-2 2-2h6a1 1 0 0 1 0 2H4v14h14v-6z"/></svg>
            <span>Edit</span>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import dayjs from 'dayjs';
import relativeTimePlugin from 'dayjs-ext/plugin/relativeTime';

export default {
  data: () => ({
    article: [],
    articleId: null,

    title: '',
    description: ''
  }),

  async mounted () {
    const response = await this.$axios.get('articles');
    this.article = response.data;
    this.articleId = response.data.id;

    const toISOString = (value) => {
      const date = new Date(value);

      const year = date.getFullYear();
      const month = date.getMonth();
      const day = date.getDate();
      const hours = date.getHours();
      const minutes = date.getMinutes();
      const seconds = date.getSeconds();

      const utcDate = Date.UTC(year, month, day, hours, minutes, seconds);

      return new Date(utcDate).toISOString();
    };
    dayjs.extend(relativeTimePlugin);
    Vue.filter('formatDate', (value) => dayjs(toISOString(value)).fromNow());
  },

  methods: {
    async postArticle() {
      if (!this.$auth.loggedIn) {
        this.$router.push('login');
      }

      const response = await this.$axios.post('articles', {
        title: this.title,
        description: this.description,
      })

      this.article.unshift(response.data);
      this.title = '';
      this.description = '';
    },

    async deleteArticle(getId) {
      if (!this.$auth.loggedIn) {
        this.$router.push('login');
      }

      await this.$axios.delete(`articles/${getId}`);

      const index = this.article.findIndex((article) => article.id == getId);
      this.article.splice(index, 1);
    },
  }
}
</script>

<style lang="css" scoped>
</style>
