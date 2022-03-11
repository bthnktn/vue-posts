<template>
  <v-app dark>
    <v-app-bar :clipped-left="clipped" fixed app>
      <NuxtLink
        style="text-decoration: none; font-weight: bold; font-size: 2rem"
        to="/"
        ><v-toolbar-title style="color: white" v-text="title"
      /></NuxtLink>
      <v-spacer />
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/',
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Posts',
          to: '/posts',
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Posts',
    }
  },
}
</script>
