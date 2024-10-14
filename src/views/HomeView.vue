<script setup>
import { ref } from "vue";
import axios from "axios";
const products = ref([]);

import ProductCard from "@/components/ProductCard.vue";
import Pagination from "@/components/Pagination.vue";

products.value = await axios
  .get("http://localhost:3000/products")
  .then((res) => res.data);
console.log(products);

// async function getProducts() {
//   const response = await axios.get("http://localhost:3000/products");
//   console.log(response.data);
//   products.value = response.data;
// }
getProducts();
</script>

<template>
  <main>
    <div class="product-grid">
      <ProductCard
        v-for="product in products"
        :key="product.id"
        :product="product"
      />
    </div>
    <div class="pagination">
      <Pagination />
    </div>
  </main>
</template>
<style scoped>
.product-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 20px;
  width: 80%;
  margin: 0 auto;
}
.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
}
</style>
