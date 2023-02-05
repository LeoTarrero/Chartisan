<template>
  <v-app>
    <navBar />
    <v-main class="grey lighten-3">
      <v-toolbar dense flat>
        <v-tabs v-model="tab" centered class="ml-n9" color="grey darken-1" fixed-tabs>
          <v-tab v-for="link in tabLinks" :key="link">
            {{ link }}
          </v-tab>
        </v-tabs>
      </v-toolbar>
      <v-container>
        <v-row>
          <v-col cols="12" sm="9">
            <v-sheet min-height="75vh" rounded="lg">
              <v-container>
                <v-row>
                  <v-col align="center">
                    <v-tabs-items v-model="tab">
                      <v-tab-item>
                        <chartTemplate />
                      </v-tab-item>
                      <v-tab-item>
                        <chartTable />
                      </v-tab-item>
                    </v-tabs-items>
                  </v-col>
                </v-row>
              </v-container>
            </v-sheet>
          </v-col>

          <v-col cols="12" sm="3">
            <v-sheet rounded="lg" height="65vh">
              <v-list>
                <chartSelector />
              </v-list>
            </v-sheet>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>
<script>
export default {
  data() {
    return {
      tab: null,
      tabLinks: ["Chart", "Table"],
    };
  },
};
</script>
