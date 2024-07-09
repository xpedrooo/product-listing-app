<!-- src/App.vue -->
<template>
  <v-app>
    <v-main>
      <product-search @search-products="filterProducts"></product-search>
      <v-btn color="primary" @click="newProduct">New Product</v-btn>
      <product-list
        :products="filteredProducts"
        @edit-product="editProduct"
        @delete-product="deleteProduct"
      ></product-list>
      <product-form ref="productForm" @save-product="saveProduct"></product-form>
    </v-main>
  </v-app>
</template>

<script>
import ProductList from './components/ProductList.vue';
import ProductSearch from './components/ProductSearch.vue';
import ProductForm from './components/ProductForm.vue';
import axios from 'axios';

export default {
  components: {
    ProductList,
    ProductSearch,
    ProductForm,
  },
  data() {
    return {
      products: [],
      filteredProducts: [],
    };
  },
  created() {
    this.fetchProducts();
  },
  methods: {
    async fetchProducts() {
      try {
        const response = await axios.get('https://fakestoreapi.com/products');
        this.products = response.data;
        this.filteredProducts = response.data;
      } catch (error) {
        console.error('Error fetching products:', error);
      }
    },
    filterProducts(query) {
      this.filteredProducts = this.products.filter((product) =>
        product.title.toLowerCase().includes(query.toLowerCase())
      );
    },
    newProduct() {
      this.$refs.productForm.open({});
    },
    editProduct(product) {
      this.$refs.productForm.open(product);
    },
    async saveProduct(product) {
      try {
        console.log(product.id)
        if (product.id) {
          await axios.put(`https://fakestoreapi.com/products/${product.id}`, product);
        } else {
          await axios.post('https://fakestoreapi.com/products', product);
        }
        this.fetchProducts();
      } catch (error) {
        console.error('Error saving product:', error);
      }
    },
    async deleteProduct(id) {
      try {
        await axios.delete(`https://fakestoreapi.com/products/${id}`);
        this.fetchProducts();
      } catch (error) {
        console.error('Error deleting product:', error);
      }
    },
  },
};
</script>
