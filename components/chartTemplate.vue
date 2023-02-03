<template>
  <v-container>
    <GChart :type="type" :data="data" :options="options" />
    <v-divider class="my-8" align="center" />
    <v-row justify="center"> </v-row>
  </v-container>
</template>

<script>
import { GChart } from "vue-google-charts/legacy";
import { chartOptions, chartData } from "~/assets/chartData.js";
import { EventBus } from "~/plugins/event-bus";

export default {
  name: "GoogleChart",
  components: {
    GChart,
  },
  data() {
    return {
      options: chartOptions,
      data: chartData,
      type: "BarChart",
    };
  },
  created() {
    EventBus.$on("selectOption", (type) => {
      this.type = type.type;
    });
  },
};
</script>
