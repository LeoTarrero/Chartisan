<template>
  <v-app-bar app color="white" flat>
    <v-tabs centered class="ml-n9" color="grey darken-1">
      <v-tab v-for="link in links" :key="link">
        {{ link }}
      </v-tab>
    </v-tabs>
    <v-avatar class="mr-16"
      ><img src="~/static/logo.svg" alt="charts are fun"
    /></v-avatar>
  </v-app-bar>
</template>
<script>
export default {
  data: () => ({
    links: ["Chart", "Table"],
  }),
};
</script>
