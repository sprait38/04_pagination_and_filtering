<!-- eslint-disable import/no-unresolved -->
<!-- eslint-disable import/extensions -->
<!-- eslint-disable max-len -->
<template>
  <main class="content container">
    <div class="content__top content__top--catalog">
      <h1 class="content__title">
        Каталог
      </h1>
      <span class="content__info">
        152 товара
      </span>
    </div>
    <div class="content__catalog">
      <ProductFilterVue :price-from.sync="filterPriceFrom" :price-to.sync="filterPriceTo" :category-id.sync="filterCategoryId" />
      <section class="catalog">
        <ProductList :products="products" />
        <BasePaginationVue v-model="page" :count="countProducts" :per-page="productsPerPage" />
      </section>
    </div>
  </main>
</template>
<script>
import products from '@/data/products';
import ProductList from '@/components/ProductList.vue';
import BasePaginationVue from '@/components/BasePagination.vue';
import ProductFilterVue from '@/components/ProductFilter.vue';

export default {
  components: { ProductList, BasePaginationVue, ProductFilterVue },
  data() {
    return {
      filterPriceFrom: 0,
      filterPriceTo: 0,
      filterCategoryId: 0,
      page: 1,
      productsPerPage: 3,
    };
  },
  computed: {
    filteredProducts() {
      let filteredProducts = products;

      if (this.filterPriceFrom > 0) {
        // eslint-disable-next-line max-len
        filteredProducts = filteredProducts.filter((product) => product.price > this.filterPriceFrom);
      }

      if (this.filterPriceTo > 0) {
        filteredProducts = filteredProducts.filter((product) => product.price < this.filterPriceTo);
      }

      if (this.filterCategoryId > 0) {
        // eslint-disable-next-line max-len
        filteredProducts = filteredProducts.filter((product) => product.categoryid === this.filterCategoryId);
      }

      return filteredProducts;
    },
    products() {
      const offset = (this.page - 1) * this.productsPerPage;
      return this.filteredProducts.slice(offset, offset + this.productsPerPage);
    },
    countProducts() {
      return this.filteredProducts.length;
    },
  },
};
</script>
