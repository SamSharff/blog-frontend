<script>
import axios from "axios";

export default {
  data: function () {
    return {
      post: {},
    };
  },
  created: function () {
    axios.get("http://localhost:3000/posts/1.json").then((response) => {
      console.log(response.data);
      this.post = response.data;
    });
  },
};
</script>

<template>
  <div>
    <h2>Post</h2>
    <p>{{ post }}</p>
  </div>
  <template />
</template>
