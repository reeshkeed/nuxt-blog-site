<template lang="html">
  <div class="">
    <div class="flex flex-col w-full">
      <div
       class="border border-gray-200 bg-white w-full rounded shadow text-left px-4 py-2 my-2"
        v-for="(list, i) in article"
        :key="i"
      >
        <div>
          <h1 class="font-bold">{{ list.title }}</h1>
          <p>{{ list.description }}</p>
          <p class="text-gray-600 text-sm">{{ list.created_at | formatDate }}</p>
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
  }),

  async mounted () {
    const response = await this.$axios.get('articles');
    this.article = response.data;

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
  }
}
</script>

<style lang="css" scoped>
</style>
