<template>
     <ul>
        <li v-for="product in products" :key="product.name">
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
</template>

<script>
  import ProductGridItem from './ProductGridItem.vue'

  export default {
    name: 'ProductGrid',
    props: ['products'],
    components: {
      ProductGridItem,
    }
  }
</script>
