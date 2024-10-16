<script setup>
import { ref, onMounted, watchEffect } from "vue";
import axios from "axios";

import ProductCard from "@/components/ProductCard.vue";
import Pagination from "@/components/Pagination.vue";
import Loading from "@/components/Loading.vue";
import ProductForm from "@/components/ProductForm.vue";

const products = ref([]);
const page = ref(1);
const limit = ref(8);
const isLoading = ref(true);

watchEffect(() => {
  fetchData();
});

async function fetchData() {
  const API_URL = `http://localhost:3000/products?_page=${page.value}&_per_page=${limit.value}`;
  try {
    isLoading.value = true;
    const respone = await axios.get(API_URL);
    products.value = respone.data;
  } catch (error) {
    console.log(error);
  } finally {
    isLoading.value = false;
  }
}
async function createProduct(product) {
  try {
    await axios.post("http://localhost:3000/products", product);
    fetchData();
  } catch (error) {
    console.log(error);
  }
}

function changePage(newPage) {
  if (newPage < 1 || newPage > products.value.pages) return;
  page.value = newPage;
}
</script>

<template>
  <div v-if="isLoading">
    <Loading />
  </div>
  <main v-else>
    <ProductForm @create-product="createProduct"/>
    <div class="product-grid">
      <ProductCard
        v-for="product in products.data"
        :key="product.id"
        :product="product"
      />
    </div>
    <div class="pagination">
      <Pagination
        :page="page"
        :totalPage="products.pages"
        @changePage="changePage"
      />
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
