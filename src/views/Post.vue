<template>

  <div class="about">
    <br>
    <p>User: {{ user.name }}</p>
    <br>
    <h1 style="text-transform: uppercase">{{ post.title }}</h1>
    <br>
    <div>
      {{ post.body }}
      <br><br>
      <p>Comments: </p>
      <br>
      <ul>
        <li v-for="coment in coments">{{ coment.body }}</li>
      </ul>
    </div>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        post: '',
        coments: '',
        user: '',
      }
    },
    created() {
      this.getPosts()
    },
      methods: {
        getPosts() {

          fetch('https://jsonplaceholder.typicode.com/posts/' + this.$route.params.id)
              .then((response) => response.json())
              .then((json) => {
                this.post = json
                fetch('https://jsonplaceholder.typicode.com/users/' + json.userId)
                    .then((response) => response.json())
                    .then((json) => this.user = json)
              });
          fetch('https://jsonplaceholder.typicode.com/posts/' + this.$route.params.id + '/comments')
              .then((response) => response.json())
              .then((json) => this.coments = json);

        }
      }
  }
</script>
<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
  }
}
</style>
