<template>
  <section class="container mx-auto">
    <div class=" flex flex-col w-full px-16">
      <div
       class="border border-gray-400 w-full rounded text-left px-4 py-2 m-2"
        v-for="(list, i) in article"
        :key="i"
      >
        <div>
          <p>{{ list.title }}</p>
          <p>{{ list.description }}</p>
          <p>{{ list.created_at | formatDate }}</p>
        </div>

      </div>
    </div>
  </section>
</template>

<script>
import AppLogo from '~/components/AppLogo.vue';
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
  },

  components: {
    AppLogo
  }
}
</script>

<style>
.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
