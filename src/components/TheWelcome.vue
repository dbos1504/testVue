<script>

import axios from 'axios'

export default {
  data() {
    return {
      posts: '',
      search: ''
    }
  },
  created() {
    this.getPosts()
  },
  methods: {
    async getPosts() {
      await fetch('https://jsonplaceholder.typicode.com/posts')
          .then((response) => response.json())
          .then(async (json) => {
            this.posts = json
            this.posts.filter(async post => {
              await fetch('https://jsonplaceholder.typicode.com/posts/' + post.id + '/comments')
                  .then((response) => response.json())
                  .then((json) => post.coments = json);
              await fetch('https://jsonplaceholder.typicode.com/posts/' + post.id)
                  .then((response) => response.json())
                  .then((json) => {
                    fetch('https://jsonplaceholder.typicode.com/users/' + post.userId)
                        .then((response) => response.json())
                        .then((json) => post.users = json)
                  });
            })
          });
    }
  },
  computed: {
    filterPosts() {
      if (this.posts) {
        return this.posts.filter(post => {
          return post.users.name.toLowerCase().includes(this.search.toLowerCase());
        });
      }
    }
  }
}
</script>

<template>
    <section>
      <br><br>
      <div>
        <input v-model="search" style="padding: 5px 10px;width: 100%" type="text" placeholder="filter posts...">
      </div>
      <br>
        <ul>
          <li v-for="post in filterPosts">
            {{ post.users ? post.users.name : '' }} wrote:
            <a style="text-transform: capitalize" :href="'/post/' + post.id">
              {{ post.title }}
            </a> <br>
            Comments: <br>
            <ul>
              <li v-for="comment in post.coments">{{ comment.name}}</li>
            </ul>
            <br>
          </li>
        </ul>
    </section>
</template>
