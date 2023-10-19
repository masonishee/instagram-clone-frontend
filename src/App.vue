<script>
import axios from "axios";
export default {
  data: function () {
    return {
      posts: [],
      newPostParams: {},
    };
  },
  created: function () {
    this.indexPosts();
  },
  methods: {
    indexPosts: function () {
      axios.get("/posts.json").then((response) => {
        console.log("posts index", response);
        this.posts = response.data;
      });
    },
    createPost: function () {
      axios
        .post("/posts", this.newPostParams)
        .then((response) => {
          console.log("posts create", response);
          this.posts.push(response.data);
          this.newPostParams = {};
        })
        .catch((error) => {
          console.log("posts create error", error.response);
        });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>New Post</h1>
    <div>
      User:
      <input type="text" v-model="newPostParams.user" />
      Image:
      <input type="text" v-model="newPostParams.image_url" />
      Comment:
      <input type="text" v-model="newPostParams.comment" />

      <button v-on:click="createPost()">Create Post</button>
    </div>
    <h1>All Posts</h1>
    <div v-for="post in posts" v-bind:key="post.id">
      <h2>{{ post.user }}</h2>
      <img v-bind:src="post.image_url" v-bind:alt="post.user" />
      <p>Comment {{ post.comment }}</p>
    </div>
  </div>
</template>

<style></style>
