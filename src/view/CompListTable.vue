<template>
  <div>
    <div class="flex">
      <router-link to="/">Giao diện dạng lưới</router-link>
      <router-link to="/Table">Giao diện dạng bảng</router-link>
    </div>
    <v-card>
      <v-card-title>
        Nutrition
        <v-spacer></v-spacer>
        <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          label="Search"
          single-line
          hide-details
        ></v-text-field>
      </v-card-title>
      <v-data-table
        :headers="headers"
        :items="desserts"
        :search="search"
        style="text-align: center"
      >
        <template v-slot:item.image="{ item }">
          <img :src="item.image" alt="Pokemon" height="50px" width="50px" />
        </template>
        <template v-slot:item.type="{ item }">
          <v-btn
            :class="index === 0 ? 'primary' : 'purple'"
            variant="outlined"
            size="small"
            v-for="(type, index) in item.types"
            :key="index"
            >{{ type }}</v-btn
          >
        </template>
        <template v-slot:item.action="{ item }">
          <v-btn small @click="performAction(item)">Action</v-btn>
        </template>
      </v-data-table>
    </v-card>
  </div>
</template>

<script>
import { getTableProduct } from "../api/api";

export default {
  data() {
    return {
      search: "",
      headers: [
        { text: "ID", value: "id" },
        { text: "IMAGE", value: "image" },
        { text: "NAME", value: "name" },
        { text: "TYPE", value: "type" },
        { text: "ACTION", value: "action" }
      ],
      desserts: []
    };
  },
  mounted() {
    this.loadProducts();
  },
  methods: {
    loadProducts() {
      getTableProduct()
        .then(products => {
          this.desserts = products;
        })
        .catch(error => {
          console.error("Error loading products:", error);
        });
    },
    performAction(item) {
      console.log("Perform action for item:", item);
    }
  }
};
</script>

<style scoped>
.primary {
  background-color: green !important;
  margin-right: 10px;
}
.purple {
  background-color: purple !important;
  color: white;
}
.flex {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  margin: 24px 36px;
  gap: 24px;
}
.flex a {
  text-decoration: none;
  background: purple;
  padding: 8px 10px;
  color: #fff;
  border-radius: 8px;
}
</style>
