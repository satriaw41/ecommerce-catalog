<template>
  <div id="app">
    <!-- Tampilkan komponen-komponen lainnya sesuai kebutuhan -->
    <MenSection v-if="currentCategory === 'men'" :products="products" />
    <WomenSection v-else-if="currentCategory === 'women'" :products="products" />
    <UnavailableProduct v-else :products="products" />
    <button @click="nextProduct">Next Product</button>
  </div>
</template>

<script>
import MenSection from './components/MenSection.vue';
import WomenSection from './components/WomenSection.vue';
import UnavailableProduct from './components/UnavailableProduct.vue';

export default {
  components: {
    MenSection,
    WomenSection,
    UnavailableProduct,
  },
  data() {
    return {
      products: [],
      currentIndex: 1,
      currentCategory: '',
    };
  },
  methods: {
    async fetchProducts() {
      try {
        const response = await fetch(`https://fakestoreapi.com/products/${this.currentIndex}`);
        const data = await response.json();

        // Cek kategori produk
        if (data.category === "men's clothing") {
          this.currentCategory = 'men';
        } else if (data.category === "women's clothing") {
          this.currentCategory = 'women';
        } else {
          this.currentCategory = 'unavailable';
        }

        // Simpan data sesuai kategori
        if (this.currentCategory === 'men' || this.currentCategory === 'women') {
          this.products.push(data);
        }
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    },
    nextProduct() {
      // Atur index kembali ke 1 setelah mencapai 20
      this.currentIndex = this.currentIndex < 20 ? this.currentIndex + 1 : 1;
      this.fetchProducts();
    },
  },
  mounted() {
    // Panggil fetchProducts saat komponen di-mount
    this.fetchProducts();
  },
};
</script>

<style>
/* Tambahkan styling sesuai kebutuhan */
</style>
