<script>
import axios from "axios";

export default {
  data: function () {
    return {
      posts: [],
    };
  },
  created: function () {
    axios.get("http://localhost:3000/posts.json").then((response) => {
      this.posts = response.data;
      console.log("All posts", this.posts);
    });
  },
  methods: {},
};
</script>

<template>
  <div v-for="post in posts" v-bind:key="post.id">
    <div v-bind:class="{ selected: post === currentPost }">
      <h2>Title: {{ post.title }}</h2>
      <img :src="post.image" v-bind:alt="post.title" />
      <p>Body: {{ post.body }}</p>
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
