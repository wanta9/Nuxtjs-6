<script lang="ts" setup>
import { useProductsStore } from "~/stores/products";

const productStore = useProductsStore();
const allProducts = ref([]);

productStore.getAllProducts().then(() => {
  allProducts.value = productStore.products;
});

const selectedCategory = ref("");

const categoryStore = useCategoryStore();
const { categories } = storeToRefs(categoryStore);
categoryStore.getAllCategory();

const sortListProduct = computed(() => {
  if (!selectedCategory.value) {
    return allProducts.value;
  } else {
    // ignore the fkin error here !
    return allProducts.value.filter(
      (product) => product.category === selectedCategory.value
    );
  }
});
</script>
<template>
  <section>
    <div class="container">
      <div class="py-10">
        <div class="mb-8 flex justify-end gap-6 pr-10">
          <NuxtLink
            to="/category/create"
            class="bg-orange-500 text-white flex justifycenter items-center px-3 rounded-lg"
            >Create Category</NuxtLink
          >
          <NuxtLink
            to="/product/create"
            class="bg-green-500 text-white flex justify-center items-center px-3 rounded-lg"
            >Create Products</NuxtLink
          >
          <Dropdown @selected-category="selectedCategory = $event" />
        </div>
        <br />
        <br />
        <div class="flex gap-10 flex-wrap ml-28">
          <template v-for="(item, index) in sortListProduct" :key="index">
            <CardsCardProduct :product="item" class="w-[calc(83%/4)]" />
          </template>
        </div>
      </div>
    </div>
    <br />
    <br />
    <br />
    <br />
    <br />
  </section>
</template>
