<template>
  <v-card
    style="height: 100%; display: grid; grid-template-rows: auto 1fr auto"
  >
    <v-card-text>
      <p class="text-h6 text--primary">{{ title }}</p>
      <div class="text--primary">{{ body }}</div>
    </v-card-text>
    <v-spacer></v-spacer>
    <v-card-actions>
      <v-btn
        text
        color="teal accent-4"
        @click="
          $router.push({
            path: `/posts/${id}`,
            params: {
              title: title,
              body: body,
            },
          })
        "
      >
        More
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  props: {
    id: {
      type: Number,
      default: -1,
    },
    title: {
      type: String,
      default: '',
    },
    body: {
      type: String,
      default: '',
    },
  },
}
</script>