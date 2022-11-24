<template>
  <v-app>
    <header-view />
    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
import HeaderView from '../src/components/shared/Header';

export default {
  name: 'App',
  components: {
    HeaderView
  },
  data: () => ({
    //
  })
};
</script>
