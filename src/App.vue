<template>
  <div id="app">
    <PostsList />
  </div>
</template>

<script>
import PostsList from '@/components/PostsList.vue';

export default {
  components: {
    PostsList
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
