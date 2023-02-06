<template>
  <v-container>
    <GChart :type="type" :data="data" :options="options" />
    <v-divider class="mb-16" align="center" />
    <v-row justify="center">
      <v-toolbar flat>
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
            label="X"
            clearable
            color="success"
          ></v-text-field>
        </v-col>
        <v-col>
          <v-text-field
            v-model="options.vAxis.title"
            label="Y"
            clearable
            color="success"
          ></v-text-field>
        </v-col>
        <v-col cols="2">
          <v-menu v-model="showMenu" style="max-width: 600px" close-on-click top>
            <template #activator="{ on, attrs }">
              <v-btn class="mb-8" icon :value="4" v-bind="attrs" tile v-on="on">
                <v-icon color="grey">mdi-format-color-fill</v-icon>
              </v-btn>
            </template>
            <v-list>
              <v-list-item-group>
                <v-list-item
                  v-for="group in colorTheme"
                  :key="group.color"
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
            </v-list> </v-menu
        ></v-col>
      </v-toolbar>
    </v-row>
  </v-container>
</template>

<script>
import { GChart } from "vue-google-charts/legacy";
import { EventBus } from "~/plugins/event-bus";
import { colorTheme } from "~/assets/chartSettings.js";
export default {
  name: "GoogleChart",
  components: {
    GChart,
  },
  props: {
    data: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      showMenu: false,
      colorTheme,
      options: {
        title: "Dragon Ball Chunks (and Krillin)",
        hAxis: {
          title: "",
          minValue: 0,
        },
        vAxis: {
          title: "",
        },
        fontName: "",
        fontSize: "16",
        width: 1000,
        height: 600,
        colors: ["#3fc7ad", "#2f9480", "#0f2e28", "#1f6154"],
      },
      // data: chartData,
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
