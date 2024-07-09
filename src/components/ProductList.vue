<!-- src/components/ProductList.vue -->
<template>
  <v-container>
    <v-row>
      <v-col v-for="product in products" :key="product.id" cols="12" md="4">
        <v-card>
          <v-img :src="product.image" aspect-ratio="1.7"></v-img>
          <v-card-title>{{ product.title }}</v-card-title>
          <v-card-subtitle>${{ product.price }}</v-card-subtitle>
          <v-card-actions>
            <v-btn color="primary" @click="editProduct(product)">Edit</v-btn>
            <v-btn color="error" @click="deleteProduct(product.id)">Delete</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      products: [],
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
      } catch (error) {
        console.error('Error fetching products:', error);
      }
    },
    editProduct(product) {
      this.$emit('edit-product', product);
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
