<script>
import axios from "axios";
export default {
  data: function () {
    return {
      posts: [],
      newPostParams: {},
      editPostParams: {},
      currentPost: {},
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
        .post("/posts.json", this.newPostParams)
        .then((response) => {
          console.log("posts create", response);
          this.posts.push(response.data);
          this.newPostParams = {};
        })
        .catch((error) => {
          console.log("posts create error", error.response);
        });
    },
    showPost: function (post) {
      this.currentPost = post;
      this.editPostParams = post;
      document.querySelector("#post-details").showModal();
    },
    updatePost: function (post) {
      axios
        .patch("/posts/" + post.id, this.editPostParams)
        .then((response) => {
          console.log("posts update", response);
          this.currentPost = {};
        })
        .catch((error) => {
          console.log("posts update error", error.response);
        });
    },
    destroyPost: function (post) {
      axios.delete("/posts/" + post.id + ".json").then((response) => {
        console.log("posts destroy", response);
        var index = this.posts.indexOf(post);
        this.posts.splice(index, 1);
      });
    },
  },
};
</script>

<template>
  <header class="bg-gradient-to-r from-blue-500 to-purple-500 text-white py-12 relative">Home | All Posts</header>
  <div class="bg-gray-100 py-8">
    <h1>New Post</h1>
    <div>
      User:
      <input type="text" v-model="newPostParams.user" />
      Image:
      <input type="text" v-model="newPostParams.image_url" />
      Comment:
      <input type="text" v-model="newPostParams.comment" />
      <button v-on:click="createPost()" class="rounded border border-gray-300 p-2 hover:bg-gray-100">
        Create Post
      </button>
    </div>
    <h1 class="text-3xl font-semibold mb-8">All Posts</h1>
    <div v-for="post in posts" v-bind:key="post.id">
      <h2>{{ post.user }}</h2>
      <img v-bind:src="post.image_url" v-bind:alt="post.user" />
      <p>Comment {{ post.comment }}</p>
      <button v-on:click="showPost(post)" class="rounded border border-gray-300 p-2 hover:bg-gray-100">
        More info
      </button>
    </div>
    <dialog id="post-details">
      <form method="dialog">
        <h1>Post info</h1>
        <p>
          User:
          <input type="text" v-model="editPostParams.user" />
        </p>
        <p>
          Image:
          <input type="text" v-model="editPostParams.image_url" />
        </p>
        <p>
          Comment:
          <input type="text" v-model="editPostParams.comment" />
        </p>
        <button v-on:click="updatePost(currentPost)" class="rounded border border-gray-300 p-2 hover:bg-gray-100">
          Update
        </button>
        <button v-on:click="destroyPost(currentPost)" class="rounded border border-gray-300 p-2 hover:bg-gray-100">
          Destroy Post
        </button>
        <button class="rounded border border-gray-300 p-2 hover:bg-gray-100">Close</button>
      </form>
    </dialog>
  </div>
  <footer class="text-center bg-gradient-to-r from-blue-500 to-purple-500 text-white py-12 relative">
    Copyright ARRAY TO REMEMBER 2023
  </footer>
</template>

<style></style>
