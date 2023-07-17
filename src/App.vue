<template>
  <div id="app">
    <h1 style="text-align: center">Post List</h1>
    <PostList />
  </div>
</template>

<script>
import PostList from "./components/PostList";
export default {
  components: {
    PostList,
  },
};
</script>