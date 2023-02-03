<template>
  <v-row justify="center">
    <v-col>
      <v-card flat width="100%">
        <v-list>
          <v-list-item-group mandatory>
            <v-list-item
              v-for="option in options"
              :key="option.title"
              @click.stop="selectOption(option.type)"
            >
              <v-list-item-icon>
                <v-icon>{{ option.icon }}</v-icon>
              </v-list-item-icon>
              <v-list-item-content>
                <v-list-item-title>{{ option.title }}</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list-item-group>
        </v-list>
      </v-card>
    </v-col>
  </v-row>
</template>
<script>
import { EventBus } from "~/plugins/event-bus";
import { settings } from "~/assets/chartSettings.js";
export default {
  data() {
    return {
      dialog: false,
      options: settings,
    };
  },
  methods: {
    selectOption(type) {
      EventBus.$emit("selectOption", { type });
    },
  },
};
</script>
