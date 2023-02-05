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
        <v-btn outlined color="success" @click="addItem">New Item</v-btn>
        <v-divider vertical class="mx-4" />
        <v-btn outlined color="success" @click="downloadData"
          ><v-icon>mdi-download</v-icon></v-btn
        >
      </v-toolbar>
      <v-data-table :headers="headers" :items="items" :search="search">
        <template #body="{ items }">
          <tbody>
            <tr v-for="item in items" :key="item.id">
              <td v-for="header in headers" :key="header">
                <v-edit-dialog
                  :return-value.sync="item[header.value]"
                  @open="open"
                  @close="close"
                >
                  {{ item[header.value] }}
                  <template #input>
                    <v-text-field
                      :key="`${header.text}-${index}`"
                      v-model="item[header.text]"
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
import { chartData } from "~/assets/chartData";

export default {
  data() {
    return {
      dialog: false,
      search: "",
      headers: chartData[0].map((header, index) => ({
        text: header,
        value: header,
      })),
      items: chartData.slice(1).map((item) => {
        const itemObject = {};
        chartData[0].forEach((header, index) => {
          itemObject[header] = item[index];
        });
        return itemObject;
      }),
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
        name: "",
      };
      this.items.push(newItem);
    },
    deleteItem(item) {
      this.editedIndex = this.items.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.items.splice(this.editedIndex, 1);
    },
    downloadData() {
      const data = "module.exports = " + JSON.stringify(this.items);
      const file = new Blob([data], { type: "application/javascript" });
      const a = document.createElement("a");
      a.href = URL.createObjectURL(file);
      a.download = "data.js";
      a.click();
    },
  },
};
</script>
