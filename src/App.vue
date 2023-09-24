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
        <label for="checkbox">Adidas</label>
        <input
          @click="toggleIsAdidas"
          type="checkbox"
          id="checkbox"
          v-model="isAdidas"
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
        <label for="checkbox">Converse</label>
        <input
          @click="toggleIsConverse"
          type="checkbox"
          id="checkbox"
          v-model="isConverse"
        />
      </div>
      <div>
        <label for="checkbox">Crocs</label>
        <input
          @click="toggleIsCrocs"
          type="checkbox"
          id="checkbox"
          v-model="isCrocs"
        />
      </div>
      
      <div>
        <label for="checkbox">New Balance</label>
        <input
          @click="toggleIsNewBalance"
          type="checkbox"
          id="checkbox"
          v-model="isNewBalance"
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
    <p>Results: {{ itemsToDisplay.length }}</p>

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
      isCrocs: false,
      isConverse: false,
      isNewBalance: false,
      brands: [],
      priceOrder: '',
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
      let products = this.products       
      let firstFiltered = this.filterByAvailability(products)
      let secondFiltered =  this.filterByBrand(firstFiltered)
      return this.filterByPrice(secondFiltered)

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
