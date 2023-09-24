<template>
  <div class="app" ref="app-container">
    <div class="title">Product Listing Page</div>

    <div class="filters">
      <h3>Filters</h3>
      <div class="in-stock-checkbox">
        <Checkbox @click="toggleIsAvailable" text="In Stock"/>
      </div>
      <div class="adidas-checkbox">
        <Checkbox @click="toggleIsAdidas" text="Adidas"/>
      </div>
      <div class="nike-checkbox">
        <Checkbox @click="toggleIsNike" text="Nike"/>
      </div>
      <div class="converse-checkbox">
        <Checkbox @click="toggleIsConverse" text="Converse"/>
      </div>
      <div class="crocs-checkbox">
        <Checkbox @click="toggleIsCrocs" text="Crocs"/>
      </div>
      <div class="new-balance-checkbox">
        <Checkbox @click="toggleIsNewBalance" text="New Balance"/>
      </div>
      
      <div>
        <label for="sortBy">Sort By: </label>
        <select v-model="sortBy">
          <option value=""></option>
          <option value="ascending">Low - High</option>
          <option value="descending">High - Low</option>
          <option value="relevance">Relevance</option>
        </select>
      </div>
    </div>
    <p>Results: {{ itemsToDisplay.length }}</p>

    <div class="product-grid">
      <ul>
        <li v-for="product in itemsToDisplay" :key="product.name">
          <ProductGridItem
            :img="product.image"
            :name="product.name"
            :price="product.price"
            :brand="product.brand"
            :rank="product.rank"
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
import Checkbox from './components/Checkbox.vue';

export default {
  name: 'App',
  components: {
    ProductGridItem, Checkbox,
  },
  props: ['products'],
  data() {
    return {
      products: products,
      isAvailable: false,
      isNike: false,
      isAdidas: false,
      isCrocs: false,
      isConverse: false,
      isNewBalance: false,
      brands: [],
      sortBy: '',
    };
  },
  methods: {
    toggleIsAvailable() {
      this.isAvailable = !this.isAvailable;
    },
    updateBrands(state, brand) {
        if (state) {
        this.brands.push(brand)
      } else {
        this.brands = this.brands.filter(item => item != brand)
      }
    },
    toggleIsNike() {
      this.isNike = !this.isNike;
      this.updateBrands(this.isNike, 'Nike')
    }, 
    toggleIsAdidas() {
      this.isAdidas = !this.isAdidas;
      this.updateBrands(this.isAdidas, 'Adidas')
    },
    toggleIsCrocs() {
      this.isCrocs = !this.isCrocs;
      this.updateBrands(this.isCrocs, 'Crocs')
    },
    toggleIsConverse() {
      this.isConverse = !this.isConverse;
      this.updateBrands(this.isConverse, 'Converse')
    },
    toggleIsNewBalance() {
      this.isNewBalance = !this.isNewBalance;
      this.updateBrands(this.isNewBalance, 'New Balance')
    },
    filterByAvailability(products) {
      return this.isAvailable
        ? this.products.filter((item) => item.isAvailable)
        : products;
    },
    filterByBrand(products) {
      let filteredBrands = []
      let items
      if (this.brands.length) {
        for (let i = 0; i < this.brands.length; i++) {
          items = products.filter((item) => item.brand === this.brands[i])
          filteredBrands = [...filteredBrands, ...items];
        }
      } else {
        return products;
      }
      return filteredBrands
    },
    sortByRelevance(products) {
        const availableProducts = products.filter((product) => product.isAvailable);
        const unavailableProducts = products.filter((product) => !product.isAvailable);
      // Sort available products by rank
        const sortedAvailableProducts = availableProducts.sort((a, b) => a.rank - b.rank);
      // Sort unavailable products by rank
        const sortedUnavailableProducts = unavailableProducts.sort((a, b) => a.rank - b.rank);
      // Concatenate available and unavailable products
        return [...sortedAvailableProducts, ...sortedUnavailableProducts];
    },
    sortOrder(products) {
      if (this.sortBy === "ascending") {
         return [...products].sort((a, b) => a.price - b.price);
      } else if (this.sortBy === "descending") {
         return [...products].sort((a, b) => b.price - a.price);
      } else if (this.sortBy === "relevance") {
        return this.sortByRelevance(products)
      } else {
         return products;
      }
    }
      
  },
  computed: {
    itemsToDisplay() {
      let products = this.products       
      let firstFiltered = this.filterByAvailability(products)
      let secondFiltered =  this.filterByBrand(firstFiltered)
      return this.sortOrder(secondFiltered)
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
