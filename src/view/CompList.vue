<template>
  <div>
    <CompHeader />

    <div class="flex">
      <router-link to="/">Giao diện dạng lưới</router-link>
      <router-link to="/Table">Giao diện dạng bảng</router-link>
    </div>
    <div class="product-list">
      <div
        class="product"
        v-for="product in paginatedData"
        :key="product.id"
        @click="showProductDetails(product.id)"
      >
        <div class="img">
          <img :src="product.image" :alt="product.name" />
        </div>
        <div class="name">
          <h3>{{ product.name.toUpperCase() }}</h3>
          <h3>#{{ product.id }}</h3>
        </div>
      </div>
    </div>
    <div class="text-center">
      <v-pagination
        v-model="page"
        :length="4"
        prev-icon="mdi-menu-left"
        next-icon="mdi-menu-right"
        class="custom-pagination"
        @input="changePage"
      ></v-pagination>
    </div>
  </div>
</template>

<script>
import { fetchProducts } from "../api/api";
import CompHeader from "../components/CompHeader.vue";
export default {
  name: "List",
  components: {
    CompHeader
  },
  data() {
    return {
      page: 1,
      itemsPerPage: 12,
      products: []
    };
  },
  mounted() {
    this.fetchProducts();
  },
  methods: {
    changePage(page) {
      this.page = page;
    },
    fetchProducts() {
      fetchProducts()
        .then(products => {
          this.products = products;
        })
        .catch(error => {
          console.error("Có lỗi khi gọi API", error);
        });
    },
    showProductDetails(id) {
      this.$router.replace({ path: `/product/${id}` });
    }
  },
  computed: {
    totalPages() {
      return Math.ceil(this.products.length / this.itemsPerPage);
    },
    paginatedData() {
      const startIndex = (this.page - 1) * this.itemsPerPage;
      const endIndex = startIndex + this.itemsPerPage;
      return this.products.slice(startIndex, endIndex);
    }
  }
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@100;200;300;400;500;600;700;800;900&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700;800;900&display=swap");
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-family: "poppins", sans-serif;
}
.product-list {
  display: grid;
  grid-template-columns: repeat(4, 1fr); /* Chia thành 4 cột bằng nhau */
  gap: 24px; /* Khoảng cách giữa các cột */
  margin: 48px 24px 20px;
}

/* Điểm dừng (breakpoints) cho responsive design */
@media (min-width: 1024px) {
  .product-list {
    grid-template-columns: repeat(4, 1fr);
  }
}

/* Chia 2 cột trên tablet */
@media (max-width: 1023px) and (min-width: 768px) {
  .product-list {
    grid-template-columns: repeat(2, 1fr);
  }
}

/* Chia 1 cột trên di động */
@media (max-width: 767px) {
  .product-list {
    grid-template-columns: repeat(1, 1fr);
  }
}
.name {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 10px;
}
.product {
  transition: transform 0.3s ease;
  padding: 10px;
  /* border: 1px solid #ccc; */
  border-radius: 10px;
  background-color: #fff;
  text-align: center;
  box-shadow: rgba(149, 157, 165, 0.2) 0px 8px 24px;
  color: #504949;
  font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
  cursor: pointer;
}

.product:hover {
  transform: translateY(-5px);
}

.product img {
  object-fit: cover;
  width: 80%;
  height: 180px;
  max-width: 150px;
  object-fit: cover;
}

.custom-pagination {
  margin-top: 20px;
}
.v-pagination__item--active {
  background: red !important;
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
