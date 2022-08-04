<script>
import axios from "axios";
import moment from "moment";

export default {
  data: function () {
    return {
      posts: [],
      titleFilter: "LOYALTY",
    };
  },
  created: function () {
    axios.get("http://localhost:3000/posts.json").then((response) => {
      this.posts = response.data;
      console.log("All posts", this.posts);
    });
  },
  methods: {
    relativeDate: function (date) {
      return moment(date).fromNow();
    },
    filterPosts: function () {
      return this.posts.filter((post) => {
        var lowerTitle = post.title.toLowerCase();
        var lowerTitleFilter = this.titleFilter.toLowerCase();
        return lowerTitle.includes(lowerTitleFilter);
      });
    },
  },
};
</script>

<template>
  <h1>All posts</h1>
  Search:
  <input v-model="titleFilter" type="text" />
  <div v-for="post in filterPosts()" v-bind:key="post.id">
    <div v-bind:class="{ selected: post === currentPost }">
      <h2>Title: {{ post.title }}</h2>
      <img :src="post.image" v-bind:alt="post.title" />
      <p>Body: {{ post.body }}</p>
      <p>Created: {{ relativeDate(post.created_at) }}</p>
      <a v-bind:href="`/posts/${post.id}`">More info</a>
    </div>
  </div>
</template>

<style>
.selected {
  color: aliceblue;
  background-color: black;
  transition: background-color 2s ease;
}
</style>
