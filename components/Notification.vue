<template lang="html">
  <div class="">
    <div class="bg-red-100 border border-red-400 text-red-700 px-4 py-3 mb-5 rounded relative" role="alert">
      <strong class="font-bold">{{ message.error }}</strong>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Notification',
  props: ['message']
}
</script>

<style lang="css" scoped>
</style>
