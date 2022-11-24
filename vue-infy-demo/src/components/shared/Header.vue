<template>
  <v-app-bar app color="primary" dark>
    <div class="d-flex align-center">
      <h2>Vue Interview Demo</h2>
    </div>

    <v-spacer></v-spacer>
    <v-btn href="/" text>
      <v-icon>mdi-home</v-icon>
      <span class="mr-2">Home</span>
    </v-btn>
    <v-btn href="/about" text>
      <v-icon>mdi-info</v-icon>
      <span class="mr-2">About</span>
    </v-btn>
    <v-btn href="https://github.com/aakki/VueInfyDemo" target="_blank" text>
      <span class="mr-2">Github Repository</span>
      <v-icon>mdi-open-in-new</v-icon>
    </v-btn>
  </v-app-bar>
</template>

<script>
export default {
  name: 'HeaderView'
};
</script>
