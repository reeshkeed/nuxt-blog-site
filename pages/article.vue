<template lang="html">
  <section class="container mx-auto flex flex-col mt-20 px-16">
    <div class="w-full text-left mt-5">
      <form @submit.prevent="updateArticle" class="bg-white border border-indigo-200 rounded px-8 pt-6 pb-8 mb-4">
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
          <button class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="submit">
            SAVE
          </button>
        </div>
      </form>
    </div>
  </section>
</template>

<script>
export default {
  middleware: 'auth',

  data: () => ({
    title: '',
    description: '',

    data: []
  }),

  async mounted() {
    const response = await this.$axios.get(`articles/${this.$route.params.id}`);
    this.title = response.data.title;
    this.description = response.data.description;
  },

  methods: {
    async updateArticle() {
      await this.$axios.put(`articles/${this.$route.params.id}`, {
        title: this.title,
        description: this.description
      });

      this.$router.push('/');

    }
  }
}
</script>

<style lang="css" scoped>
</style>
