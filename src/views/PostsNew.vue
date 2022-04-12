<script>
import axios from "axios";

export default {
  data: function () {
    return {
      newPostParams: {},
      errors: [],
    };
  },
  methods: {
    createPost: function () {
      console.log("Making a new post");

      axios
        .post("http://localhost:3000/posts.json", this.newPostParams)
        .then((response) => {
          console.log("Sucess", response.data);
          this.posts.push(response.data);
        })
        .catch((error) => console.log(error.response));
    },
  },
};
</script>

<template>
  <div class="posts-new">
    <form v-on:submit.prevent="createPost()">
      <h1>This is the place to make a new Posts!</h1>
      <div>
        <div class="form-group">
          title:
          <input type="text" v-model="newPostParams.title" />
        </div>
        <div class="form-group">
          body:
          <input type="text" v-model="newPostParams.body" />
        </div>
        <div class="form-group">
          image:
          <input type="text" v-model="newPostParams.image" />
        </div>
      </div>
      <input type="submit" value="create" />
    </form>
  </div>
</template>
