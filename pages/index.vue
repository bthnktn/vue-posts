<template>
  <div>
    <v-row>
      <v-col cols="12">
        <v-text-field
          v-model="searchInput"
          label="Search"
          color="teal"
          @input="search()"
        ></v-text-field>
      </v-col>
    </v-row>
    <v-row justify="center">
      <v-pagination
        v-model="page"
        color="teal"
        :length="pageLength"
        :total-visible="7"
        @input="paging()"
      ></v-pagination>
    </v-row>
    <v-row>
      <v-col v-for="post of pagePosts" :key="post.id" lg="4" sm="6" cols="12">
        <Card :id="post.id" :title="post.title" :body="post.body" />
      </v-col>
    </v-row>

    <v-dialog
      v-model="addPost"
      max-width="600px"
      overlay-opacity="0.8"
      @click:outside="clear()"
    >
      <template #activator="{ on, attrs }">
        <v-fab-transition>
          <v-btn
            v-show="!hidden"
            color="teal"
            fab
            dark
            fixed
            bottom
            right
            v-bind="attrs"
            v-on="on"
          >
            <v-icon>mdi-plus</v-icon>
          </v-btn>
        </v-fab-transition>
      </template>
      <v-card>
        <v-card-title>
          <span class="text-h5">Add Post</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12">
                <v-text-field
                  v-model="addPostTitle"
                  label="Title"
                  required
                  color="teal"
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-textarea
                  v-model="addPostBody"
                  label="Body"
                  required
                  outlined
                  color="teal"
                ></v-textarea>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="teal darken-1" text @click="clear()"> Close </v-btn>
          <v-btn color="teal darken-1" text @click="submit()"> Add </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import Card from '../components/Card.vue'
const postPerPage = 12
export default {
  components: { Card },
  data() {
    return {
      posts: [],
      filteredPosts: [],
      searchInput: '',
      page: 1,
      pageLength: 1,
      pagePosts: [],
      addPost: false,
      addPostTitle: '',
      addPostBody: '',
    }
  },
  async mounted() {
    this.posts = await fetch('https://jsonplaceholder.typicode.com/posts')
      .then((res) => res.json())
      .then((res) => (this.filteredPosts = res))

    this.pageLength = Math.ceil(this.filteredPosts.length / postPerPage)
    this.paging()
  },
  methods: {
    search() {
      this.filteredPosts = this.posts.filter((post) =>
        post.title.toLowerCase().includes(this.searchInput.toLowerCase())
      )
      this.pageLength = Math.ceil(this.filteredPosts.length / postPerPage)
      this.page = 1
      this.paging()
    },
    paging() {
      this.pagePosts = this.filteredPosts.slice(
        (this.page - 1) * postPerPage,
        this.page * postPerPage
      )
    },
    async submit() {
      await fetch('https://jsonplaceholder.typicode.com/posts', {
        method: 'POST',
        body: JSON.stringify({
          title: this.addPostTitle,
          body: this.addPostBody,
          userId: 1,
        }),
        headers: {
          'Content-type': 'application/json; charset=UTF-8',
        },
      })
        .then((res) => res.json())
        .then((json) => {
          console.log(json)
          this.clear()
        })
    },
    clear() {
      this.addPost = false
      this.addPostTitle = ''
      this.addPostBody = ''
    },
  },
}
</script>
