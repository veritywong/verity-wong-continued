<template>
  <div class="app" ref="app-container">
    <div class="title">Product Listing Page</div>

    <div class="filters">
      <h3>Filters</h3>
      <div>
        <label for="checkbox">In Stock</label>
        <input
          @click="toggleIsAvailable"
          type="checkbox"
          id="checkbox"
          v-model="isAvailable"
        />
      </div>
      <div>
        <label for="checkbox">Nike</label>
        <input
          @click="toggleIsNike"
          type="checkbox"
          id="checkbox"
          v-model="isNike"
        />
      </div>
      <div>
        <label for="checkbox">Adidas</label>
        <input
          @click="toggleIsAdidas"
          type="checkbox"
          id="checkbox"
          v-model="isAdidas"
        />
      </div>
      <div>
        <label for="priceOrder">Sort By Price </label>
        <select v-model="priceOrder">
          <option value=""></option>
          <option value="ascending">Low - High</option>
          <option value="descending">High - Low</option>
        </select>
      </div>
    </div>
    <!-- <p>Results: {{ itemsToDisplay.length }}</p> -->

    <div class="product-grid">
      <ul>
        <li v-for="product in itemsToDisplay" :key="product.name">
          <ProductGridItem
            :img="product.image"
            :name="product.name"
            :price="product.price"
            :brand="product.brand"
            :isAvailable="product.isAvailable"
          />
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import ProductGridItem from './components/ProductGridItem.vue';
import products from './data/products.json';

export default {
  name: 'App',
  components: {
    ProductGridItem,
  },
  props: ['products'],
  data() {
    return {
      products: products,
      isAvailable: false,
      isNike: false,
      isAdidas: false,
      brands: ['Nike', 'Adidas'],
      priceOrder: '',
    };
  },
  methods: {
    toggleIsAvailable() {
      this.isAvailable = !this.isAvailable;
    },
    toggleIsNike() {
      this.isNike = !this.isNike;
      this.brand = 'Nike'
    },
    toggleIsAdidas() {
      this.isAdidas = !this.isAdidas;
      this.brand = 'Adidas'
    },
    filterByAvailability(products) {
      return this.isAvailable
        ? this.products.filter((item) => item.isAvailable)
        : products;
    },
    // filterBrand(products) {
    //   return this.brand
    //     ? this.products.filter((item) => item.brand === this.brand)
    //     : products;
    // },
    filterByBrand(products) {

      if (this.brands.length) {
        for (let i = 0; i < this.brands.length; i++) {
          let item = products.filter((item) => item.brand === this.brands[i])
          console.log(item)
        }

      } else {
        return products;
      }
 
      // return products.filter((product) => {
      //   return !this[`is${brand}`] || product.brand === brand;
      // });
    },
    filterNike(products) {
      return this.isNike
        ? this.products.filter((item) => item.brand === 'Nike')
        : products;
    },
    filterAdidas(products) {
      return this.isAdidas
        ? this.products.filter((item) => item.brand === 'Adidas')
        : products;
    },
    filterByPrice(products) {
      if (this.priceOrder === "ascending") {
         return [...products].sort((a, b) => a.price - b.price);
      } else if (this.priceOrder === "descending") {
         return [...products].sort((a, b) => b.price - a.price);
      } else {
         return products;
      }
    },
  },
  computed: {
    itemsToDisplay() {
      let products = this.products;
      // let availableProducts = this.filterIsAvailable(products);
      // let productsByPrice = this.filterByPrice(availableProducts)
      // let nikes = this.filterNike(productsByPrice);
      // let adidas = this.filterAdidas(products);
      // console.log(this.isAvailable)
      // return this.filterAdidas(nikes);
       
       return this.filterByBrand(products)

      let firstFiltered = this.filterByAvailability(products)
      let secondFiltered =  this.filterNike(firstFiltered)
      return this.filterByPrice(secondFiltered)

      // return this.products.filter((product) => {
      //   const isAvailable = !this.isAvailable || product.isAvailable;
      //   const isNike = !this.isNike || product.brand === 'Nike';
      //   const isAdidas = !this.isAdidas || product.brand === 'Adidas';
      //   // console.log(isAvailable)
      //   // not showing anything when nike and adidas both chosen
      //   return isAvailable && isNike && isAdidas;
      // });

      // return this.filterAdidas(products).filter((product) => availableProducts.includes(product));

      // if (this.brand || this.priceOrder) {
      //   console.log(this.brand)
      //   return this.filterByPrice(this.filterNike(products));
      // }
      
      // // in stock and nike filter working togeter
      // return this.filterNike(products).filter((product) => availableProducts.includes(product));
      
      // price filter working individually
      // return this.filterByPrice(products)
    },
  },
};
</script>

<style>
.app {
  max-width: 1024px;
  margin: 0 auto;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  color: #606569;
}

.link {
  color: #3a7f71;
}
</style>
