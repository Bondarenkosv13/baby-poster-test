<template>
  <v-app>
    <v-main>
      <MainPage/>
    </v-main>
  </v-app>
</template>

<script>
import MainPage from './pages/main-page';

export default {
  name: 'App',
  components: {
    MainPage,
  },
};
</script>
