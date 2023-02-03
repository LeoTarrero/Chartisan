<template>
  <v-app id="inspire">
    <navBar />
    <v-main class="grey lighten-3">
      <v-container>
        <v-row>
          <v-col cols="12" sm="9">
            <v-sheet min-height="75vh" rounded="lg">
              <v-container>
                <v-row>
                  <v-col align="center">
                    <chartTemplate />
                  </v-col>
                </v-row>
              </v-container>
            </v-sheet>
          </v-col>

          <v-col cols="12" sm="3">
            <v-sheet rounded="lg" min-height="75vh">
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
export default {};
</script>
