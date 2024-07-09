<!-- src/components/ProductForm.vue -->
<template>
  <v-dialog v-model="dialog" max-width="500px">
    <v-card>
      <v-card-title>
        <span class="headline">{{ formTitle }}</span>
      </v-card-title>
      <v-card-text>
        <v-form ref="form" v-model="valid" lazy-validation>
          <v-text-field
            v-model="product.title"
            label="Title"
            :rules="[rules.required]"
          ></v-text-field>
          <v-text-field
            v-model="product.price"
            label="Price"
            type="number"
            :rules="[rules.required]"
          ></v-text-field>
          <v-text-field
            v-model="product.image"
            label="Image URL"
            :rules="[rules.required]"
          ></v-text-field>
        </v-form>
      </v-card-text>
      <v-card-actions>
        <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
        <v-btn color="blue darken-1" text @click="save">Save</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  data() {
    return {
      dialog: false,
      valid: false,
      product: {
        title: '',
        price: 0,
        image: '',
      },
      rules: {
        required: (value) => !!value || 'Required.',
      },
    };
  },
  computed: {
    formTitle() {
      return this.product.id ? 'Edit Product' : 'New Product';
    },
  },
  methods: {
    open(product) {
      this.product = { ...product };
      this.dialog = true;
    },
    close() {
      this.dialog = false;
    },
    save() {
      if (this.$refs.form.validate()) {
        this.$emit('save-product', this.product);
        this.close();
      }
    },
  },
};
</script>
