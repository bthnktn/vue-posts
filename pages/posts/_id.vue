<template>
  <v-row justify="center" align="center">
    <v-col lg="6" sm="10">
      <div v-if="$fetchState.pending">
        <v-skeleton-loader
          class="mx-auto my-3"
          type="article"
        ></v-skeleton-loader>
      </div>
      <div v-else class="my-4 px-10 py-3" style="background-color: #1e1e1e">
        <h1>{{ post.title }}</h1>
        <hr />
        <p class="mt-8">{{ post.body }}</p>
      </div>
      <h2>Comments</h2>
      <div v-if="$fetchState.pending">
        <v-skeleton-loader
          v-for="n in 3"
          :key="n"
          class="mx-auto my-3"
          type="article"
        ></v-skeleton-loader>
      </div>
      <div v-else>
        <v-card v-for="comment of comments" :key="comment.id" class="my-3">
          <v-card-text>
            <p class="text-h6 text--primary mb-0">
              {{ comment.name }}
            </p>
            <p>{{ comment.email }}</p>
            <div class="text--primary">{{ comment.body }}</div>
          </v-card-text>
        </v-card>
      </div>
    </v-col>
  </v-row>
</template>

<script>
export default {
  data() {
    return {
      post: {},
      comments: [],
    }
  },
  async fetch() {
    const id = this.$route.params.id

    this.post = await fetch(
      `https://jsonplaceholder.typicode.com/posts/${id}`
    ).then((res) => res.json())
    this.comments = await fetch(
      `https://jsonplaceholder.typicode.com/posts/${id}/comments`
    ).then((res) => res.json())
  },
}
</script>