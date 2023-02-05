<template>
  <v-container>
    <GChart :type="type" :data="data" :options="options" />
    <v-divider class="mb-16" align="center" />
    <v-row justify="center">
      <v-toolbar dense flat>
        <v-col>
          <v-text-field
            v-model="options.title"
            label="Chart Title"
            clearable
            color="success"
          ></v-text-field>
        </v-col>
        <v-col>
          <v-text-field
            v-model="options.hAxis.title"
            label="X Title"
            clearable
            color="success"
          ></v-text-field>
        </v-col>
        <v-col>
          <v-text-field
            v-model="options.vAxis.title"
            label="Y Title"
            clearable
            color="success"
          ></v-text-field>
        </v-col>

        <v-menu v-model="showMenu" style="max-width: 600px" close-on-click top>
          <template #activator="{ on, attrs }">
            <v-btn :value="4" text v-bind="attrs" tile v-on="on">
              <v-icon color="grey">mdi-format-color-fill</v-icon>
            </v-btn>
          </template>
          <v-list>
            <v-list-item-group>
              <v-list-item
                v-for="group in colorTheme"
                :key="group"
                mandatory
                @click="selectedColors(group.color)"
              >
                <v-avatar
                  v-for="color in group.color"
                  :key="color"
                  :style="{ backgroundColor: color }"
                  tile
                  class="my-2"
                ></v-avatar>
              </v-list-item>
            </v-list-item-group>
          </v-list>
        </v-menu>

        <div class="mx-4"></div>
      </v-toolbar>
    </v-row>
  </v-container>
</template>

<script>
import { GChart } from "vue-google-charts/legacy";
import { chartData } from "~/assets/chartData.js";
import { EventBus } from "~/plugins/event-bus";
import { colorTheme } from "~/assets/chartSettings.js";
export default {
  name: "GoogleChart",
  components: {
    GChart,
  },
  data() {
    return {
      showMenu: false,
      colorTheme,
      options: {
        title: "DragonBall Top Tanks",
        hAxis: {
          title: "Bulkiness",
          minValue: 0,
        },
        vAxis: {
          title: "Character",
        },
        fontName: "Roboto",
        fontSize: "16",
        width: 1000,
        height: 600,
        colors: ["#3fc7ad", "#2f9480", "#0f2e28", "#1f6154"],
      },
      data: chartData,
      type: "BarChart",
      watch: {
        titleInput(newTitle) {
          this.options.title = newTitle;
        },
      },
    };
  },

  created() {
    EventBus.$on("selectOption", (type) => {
      this.type = type.type;
    });
  },

  methods: {
    selectedColors(color) {
      this.options.colors = color;
    },
  },
};
</script>
