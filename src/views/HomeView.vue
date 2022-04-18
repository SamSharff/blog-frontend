<script>
import axios from "axios";
import moment from "moment";

export default {
  data: function () {
    return {
      posts: [],
      newPostTitle: "",
      newPostImage: "",
      newPostBody: { body: "" },
      currentPost: {},
      status: "",
      titleFilter: "",
    };
  },
  created: function () {
    this.indexPosts();
  },
  methods: {
    indexPosts: function () {
      axios.get("/posts").then((response) => {
        this.posts = response.data;
        console.log(this.posts);
      });
    },
    createPost: function () {
      var params = {
        title: this.newPostTitle,
        image: this.newPostImage,
        body: this.newPostBody,
      };
      axios
        .post("/posts", params)
        .then((response) => {
          this.posts.push(response.data);
          this.newPostTitle = "";
          this.newPostImage = "";
          this.newPostBody = "";
        })
        .catch((error) => {
          console.log(error.response.status, error.response.statusTest), (this.status = error.response.status);
        });
    },
    showPost: function (post) {
      if (post === this.currentPost) {
        this.currentPost = {};
      } else {
        this.currentPost = post;
      }
    },
    updatePost: function (post) {
      var params = {
        title: post.title,
        image: post.image,
        body: post.body,
      };
      axios
        .patch("/posts/" + post.id, params)
        .then((response) => {
          post.title = response.data.title;
          post.image = response.data.image;
          post.body = response.data.body;
          this.currentPost = {};
        })
        .catch((error) => {
          console.log(error.response);
        });
    },
    destroyPost: function (post) {
      axios.delete("/posts/" + post.id).then((response) => {
        console.log(response.data);
        var index = this.posts.indexOf(post);
        this.posts.splice(index, 1);
      });
    },
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
  <div class="home">
    <div>
      <h1>New Post</h1>
      Title:
      <input type="text" v-model="newPostTitle" />
      Image:
      <input type="text" v-model="newPostImage" />
      Body:
      <input type="text" v-model="newPostBody" />
      <button v-on:click="createPost()">Create</button>
      <!-- character count validation - Body -->
      <small>{{ 500 - newPostBody.body.length }} characters remaining</small>
    </div>
    <h1>All posts</h1>
    <div v-for="post in posts" v-bind:key="post.id">
      <h2>
        <p>Title:{{ post.title }}</p>
        <p>Created: {{ relativeDate(post.created_at) }}</p>

        <button v-on:click="showPost(post)">more info</button>
      </h2>
      <div v-if="post === currentPost">
        <img v-bind:src="post.image" alt="" />
        <p>{{ post.body }}</p>
        <div>
          <h4>Edit post</h4>
          Title:
          <input type="text" v-model="post.title" />
          Image:
          <input type="text" v-model="post.image" />
          Body:
          <input type="text" v-model="post.body" />
          <button v-on:click="updatePost(post)">Update</button>
          <button v-on:click="destroyPost(post)">Destroy</button>
        </div>
      </div>
    </div>
  </div>
</template>
