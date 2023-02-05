<template>
  <v-app>
    <navBar />
    <v-main class="grey lighten-3">
      <v-toolbar dense flat>
        <v-tabs v-model="tab" centered class="ml-n9" color="grey darken-1" fixed-tabs>
          <v-tab v-for="link in tabLinks" :key="link">
            {{ link }}
          </v-tab>
        </v-tabs>
      </v-toolbar>
      <v-container>
        {{items}}
        <v-row>
          
          <v-col cols="12" sm="9">
            <v-sheet min-height="75vh" rounded="lg">
              <v-container>
                <v-row>
                  <v-col align="center">
                    <v-tabs-items v-model="tab">
                      <v-tab-item>
                        <chartTemplate :data="dataItems " />
                      </v-tab-item>
                      <v-tab-item>
                        <chartTable :headers="headers" :items="items" @updateTable="updateTable"/>
                      </v-tab-item>
                    </v-tabs-items>
                  </v-col>
                </v-row>
              </v-container>
            </v-sheet>
          </v-col>

          <v-col cols="12" sm="3">
            <v-sheet rounded="lg" height="65vh">
              <v-list>
                <chartSelector />
              </v-list>
            </v-sheet>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>
<script>
import { chartData } from "~/assets/chartData";
export default {
  data() {
    return {
      tab: null,
      tabLinks: ["Chart", "Table"],
      headers: chartData[0].map((header) => ({
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
  computed:{
    dataItems(){
      const finalList = [];
      const headers = this.items[0] ? Object.keys(this.items[0]):[]
      finalList.push(headers)
      for (const item in this.items){
        const tempList = []
        for (const header in headers){
          Number(this.items[item][headers[header]]) ? tempList.push(Number(this.items[item][headers[header]])): tempList.push(this.items[item][headers[header]])
          
        }
        finalList.push(tempList)
      }
      return finalList
    }
  },
  methods:{
    updateTable(items){
      this.items = items
    }
  },
  
};
</script>
