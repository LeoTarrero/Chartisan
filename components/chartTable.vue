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
        <v-divider vertical class="mx-4" />
        <v-btn outlined color="success" @click="downloadData"
          ><v-icon>mdi-download</v-icon></v-btn
        >
        <v-divider vertical class="mx-4" />
        <v-btn outlined color="success" @click="saveTable"
          ><v-icon>mdi-save</v-icon></v-btn
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
import fs from "fs";
export default {
  data() {
    return {
      dialog: false,
      search: "",
      headers: [
        {
          text: "Character",
          align: "left",
          sortable: false,
          value: "name",
        },
        { text: "Weight", value: "weight" },
        { text: "Height", value: "height" },
      ],
      items: [],
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
    async saveTable() {
      const data = "File content to save";
      await fs.promises.writeFile("path/to/file.txt", data);
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
