<template>
  <v-app dark>
    <theme-toolbar />

    <theme-view />
  </v-app>
</template>
<script>

  export default {
    name: 'ThemeGenerator',

    components: {
      ThemeToolbar: () => import('./components/Toolbar'),
      ThemeView: () => import('./components/View')
    }
  }
</script>
