<template>
    <div>
      <h2 v-if="isNewProduct">Add Product</h2>
      <h2 v-else>Edit Product</h2>
        <form @submit.prevent="submitForm">
          <div class="mb-3">
            <label for="name" class="form-label">Name:</label>
            <input class="form-control" type="text" id="name" v-model="product.name" required />
          </div>
          <div class="mb-3">
            <label for="code" class="form-label">Code:</label>
            <input class="form-control" type="text" id="code" v-model="product.code" required />
          </div>
          <div class="mb-3">
            <label for="zip_code" class="form-label">Cep:</label>
            <input class="form-control" type="text" id="zip_code" v-model="product.zip_code" required />
          </div>
          <div class="mb-3">
            <label for="thumbnail" class="form-label">Thumbnail:</label>
            <input class="form-control" type="text" id="thumbnail" v-model="product.thumbnail" required />
            <img :src="product.thumbnail" :alt="product.name">
        </div>
          <div class="mb-3">
            <label for="price" class="form-label">Price:</label>
            <input class="form-control" type="number" id="price" v-model="product.price" required />
          </div>
          <button type="submit" v-if="isNewProduct" class="btn btn-primary">Add Product</button>
          <button type="submit" v-else class="btn btn-primary">Update Product</button>
        </form>
    </div>
  </template>

  <script>
  import axios from 'axios';
  export default {
    data() {
      return {
        product: {
          name: '',
          code: '',
          thumbnail: '',
          zip_code: '',
          price: 0
        }
      }
    },
    computed: {
      isNewProduct() {
        return !this.$route.path.includes('edit');
      }
    },
    async created() {
      if (!this.isNewProduct) {
        const response = await axios.get(`/api/products/${this.$route.params.id}`);
        this.product = response.data;
      }
    },
    methods: {
      async submitForm() {
        try {
          if (this.isNewProduct) {
            await axios.post('/api/products', this.product);
          } else {
            await axios.put(`/api/products/${this.$route.params.id}`, this.product);
          }
          this.$router.push('/');
        } catch (error) {
          console.error(error);
        }
      }
    }
  }
  </script>
