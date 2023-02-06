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
      <v-data-table :headers="localHeaders" :items="localItems" :search="search">
        <template #item="{ item, headers }">
          <tr>
            <td v-for="header in headers" :key="header.text">
              <v-text-field
                v-model="item[header.value]"
                color="success"
                hide-details
                @change="$emit('updateTable', localItems)"
              >
              </v-text-field>
            </td>
            <v-col>
              <v-icon small @click="deleteItem(item)">mdi-close</v-icon>
            </v-col>
          </tr>
        </template>
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
      localHeaders: Array.from(this.headers),
      localItems: Array.from(this.items),
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
      this.localItems.push(newItem);
    },
    deleteItem(item) {
      this.editedIndex = this.localItems.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.localItems.splice(this.editedIndex, 1);
      this.$emit("updateTable", this.localItems);
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
<style></style>
