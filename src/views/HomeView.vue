<template>
  <div class="bg-base-400">
    <h1 class="text-center text-3xl my-4">Fake Sample Shop</h1>
    <div class="grid md:grid-cols-4 grid-cols-1">
      <div class="h-fit mt-2 md:w-full w-[calc(100%-1rem)] mx-auto">
        <ul class="border-[1px] bg-white p-4">
          <li @click="getAllProducts()" class="my-2 list-none cursor-pointer">
            All
          </li>
          <li
            v-for="(category, index) in categories"
            :key="index"
            @click="getActiveCategory(category)"
            class="my-2 list-none cursor-pointer"
            :class="selectedCategory === category ? 'text-green-800 font-bold': ''"
          >
            {{ category }}
          </li>
        </ul>
      </div>
      <div class="flex flex-wrap col-span-3">
        <div class="md:w-1/4 w-full p-2 flex flex-col" v-for="product in products" :key="product.id">
          <div class="rounded-md shadow-sm border-[1px] h-full">
            <figure class="p-2">
              <img :src="product.image" />
            </figure>
            <div class="p-4">
              <h2 class="font-semibold">{{product.title}}</h2>
              <p> Rating: {{ product.rating.rate}} ({{product.rating.count}})</p>
              <p class="text-red-900"> {{product.price }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      categories: [],
      products: [],
      selectedCategory: null,
    };
  },
  methods: {
    getProducts() {
      axios
        .get(
          `https://fakestoreapi.com/products/category/${this.selectedCategory}`
        )
        .then((response) => {
          this.products = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getCategories() {
      axios
        .get("https://fakestoreapi.com/products/categories")
        .then((response) => {
          this.categories = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getActiveCategory(category) {
      this.selectedCategory = category;
      this.getProducts();
    },
    getAllProducts() {
      axios.get("https://fakestoreapi.com/products")
      .then((response)=>{
        this.products = response.data;
      })
      .catch((error)=>{
        console.log(error)
      })
    }
  },

  mounted() {
    this.getCategories();
    this.getAllProducts();
  },
};
</script>