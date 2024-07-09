<template>
  <v-text-field
    label="Search"
    v-model="searchQuery"
    @input="searchProducts"
  ></v-text-field>
</template>

<script>
export default {
  data() {
    return {
      searchQuery: '',
    };
  },
  methods: {
    searchProducts() {
      this.$emit('search-products', this.searchQuery);
    },
  },
};
</script>
