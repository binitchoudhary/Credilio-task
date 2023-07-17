<template>
  <div>
    <input type="text" v-model="searchKeyword" placeholder="Search..." />
    <div class="card-container">
      <div class="card" v-for="post in paginatedPosts" :key="post.id">
        <h3>{{ post.title }}</h3>
        <p>{{ post.body }}</p>
        <p>Author: {{ getUser(post.userId).name }}</p>
      </div>
    </div>
    <nav v-if="totalPages > 1">
      <button :disabled="currentPage === 1" @click="previousPage">
        Previous
      </button>
      <span>{{ currentPage }}</span>
      <button :disabled="currentPage === totalPages" @click="nextPage">
        Next
      </button>
    </nav>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      posts: [],
      users: [],
      currentPage: 1,
      pageSize: 10,
      searchKeyword: "",
    };
  },
  computed: {
    filteredPosts() {
      return this.posts.filter((post) =>
        post.title.toLowerCase().includes(this.searchKeyword.toLowerCase())
      );
    },
    totalPages() {
      return Math.ceil(this.filteredPosts.length / this.pageSize);
    },
    paginatedPosts() {
      const startIndex = (this.currentPage - 1) * this.pageSize;
      const endIndex = startIndex + this.pageSize;
      return this.filteredPosts.slice(startIndex, endIndex);
    },
  },

  methods: {
    async getPosts() {
      try {
        const response = await axios.get(
          "https://jsonplaceholder.typicode.com/posts"
        );
        this.posts = response.data;
      } catch (error) {
        console.error(error);
      }
    },
    async getUsers() {
      try {
        const response = await axios.get(
          "https://jsonplaceholder.typicode.com/users"
        );
        this.users = response.data;
      } catch (error) {
        console.error(error);
      }
    },
    getUser(userId) {
      return this.users.find((user) => user.id === userId) || {};
    },
    previousPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },
  },
  mounted() {
    this.getPosts();
    this.getUsers();
  },
};
</script>

<style>
.card-container {
  display: flex;
  flex-wrap: wrap;
  margin: 0 auto;
  justify-content: center;
}

.card {
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 10px;
  margin: 10px;
  width: 300px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.card h3 {
  margin-top: 0;
}

.card p {
  margin-bottom: 10px;
}

nav {
  margin-top: 10px;
  text-align: center;
}
input {
  margin-left: 10.5rem;
}
</style>