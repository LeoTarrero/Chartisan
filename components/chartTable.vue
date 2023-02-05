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
      {{items}}
      <v-data-table :headers="headers" :items="items" :search="search" >
        <template #item="{ item,headers }">
          <tr>
            <td v-for="header in headers" :key="header">
              <v-text-field
                v-model="item[header.value]"
                dense
                flat  
                solo
              >
              </v-text-field>
            </td>
          </tr>
        </template>
        <!-- <template #body="{ items }">
          <tbody>
            <tr v-for="(item, index) in items" :key="index">
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
        </template> -->
      </v-data-table>
    </v-card>
  </v-container>
</template>
<script>
export default {
  props: {
    headers: {
      type: Array,
      required: true,
    },
    items: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      dialog: false,
      search: "",
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
      /*    this.items.push(newItem); */
      this.$emit("add-item", newItem);
    },
    deleteItem(item) {
      this.editedIndex = this.items.indexOf(item);
      this.editedItem = Object.assign({}, item);
      /*     this.items.splice(this.editedIndex, 1); */
      this.$emit("update-items", this.items);
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
