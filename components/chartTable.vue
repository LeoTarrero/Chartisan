<template>
  <v-container>
    <v-card class="pa-4" outlined>
      <v-toolbar flat color="#FAFAFA">
        <v-text-field
          v-model="search"
          prepend-icon="mdi-magnify"
          label="Search"
          color="success"
          clearable
          hide-details
        ></v-text-field>
        <v-spacer />
        <v-btn outlined color="success" @click="addItem">Add Rows</v-btn>
      </v-toolbar>
      <v-data-table :headers="headers" :items="items" :search="search">
        <template #body="{ items }">
          <tbody>
            <tr v-for="item in items" :key="item">
              <td v-for="header in headers" :key="header">
                <v-edit-dialog
                  :return-value.sync="item[header.value]"
                  @open="open"
                  @close="close"
                >
                  {{ item[header.value] }}
                  <template #input>
                    <v-text-field
                      v-model="item[header.value]"
                      label="Edit"
                      single-line
                      clearable
                      color="success"
                    ></v-text-field>
                  </template>
                </v-edit-dialog>
              </td>

              <v-icon class="my-4" small @click="deleteItem(item)">
                mdi-window-close
              </v-icon>
            </tr>
          </tbody>
        </template>
      </v-data-table>
    </v-card>
  </v-container>
</template>
<script>
import { chartData } from "~/assets/chartData.js";
export default {
  data() {
    return {
      dialog: false,
      search: "",
      headers: [
        {
          text: chartData[0][0],
          align: "left",
          sortable: false,
          value: "name",
        },
        { text: chartData[0][1] + " (kg)", value: chartData[0][1] },
        { text: chartData[0][2] + " (cm)", value: chartData[0][2] },
      ],
      items: [
        { name: chartData[1][0], Height: chartData[1][1], Weight: chartData[1][2] },
        { name: chartData[2][0], Height: chartData[2][1], Weight: chartData[2][2] },
        { name: chartData[3][0], Height: chartData[3][1], Weight: chartData[3][2] },
        { name: chartData[4][0], Height: chartData[4][1], Weight: chartData[4][2] },
        { name: chartData[5][0], Height: chartData[5][1], Weight: chartData[5][2] },
      ],
    };
  },

  methods: {
    save() {},
    cancel() {},
    open() {},
    close() {},
    addItem() {
      const newItem = {
        id: this.items.length + 1,
        name: "New Character",
      };
      this.items.push(newItem);
    },
    deleteItem(item) {
      this.editedIndex = this.items.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.items.splice(this.editedIndex, 1);
    },
  },
};
</script>
