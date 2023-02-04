<template>
  <v-container>
    <v-card class="pa-4">
      <v-toolbar flat color="#FAFAFA">
        <v-text-field
          v-model="search"
          prepend-icon="mdi-magnify"
          label="Search"
          color="success"
          hide-details
        ></v-text-field>
        <v-spacer />
        <v-btn outlined color="success" @click="addItem">Add Rows</v-btn>
      </v-toolbar>
      <v-data-table :headers="headers" :items="items" :search="search" item-key="name">
        <template #body="{ items }">
          <tbody>
            <tr v-for="(item, idx) in items" :key="idx">
              <td v-for="(header, key) in headers" :key="key">
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
export default {
  data() {
    return {
      search: "",
      headers: [
        {
          text: "Name",
          align: "left",
          sortable: false,
          value: "name",
        },
        { text: "Calories", value: "calories" },
        { text: "Fat (g)", value: "fat" },
        { text: "Carbs (g)", value: "carbs" },
        { text: "Protein (g)", value: "protein" },
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
        name: "",
        calories: "",
        fat: "",
        carbs: "",
        protein: "",
        iron: "",
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
