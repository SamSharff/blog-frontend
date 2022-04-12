<script>
import axios from "axios";

export default {
  data: function () {
    return {
      post: {},
    };
  },
  created: function () {
    axios.get("http://localhost:3000/posts/" + this.$route.params.id + ".json").then((response) => {
      console.log(response.data);
      this.post = response.data;
    });
  },
};
</script>

<template>
  <div>
    <h2>Post Show</h2>
    <h3>Title: {{ post.title }}</h3>
    <p>{{ post }}</p>
    <a v-bind:href="`/posts`">Go back</a>
  </div>
</template>
