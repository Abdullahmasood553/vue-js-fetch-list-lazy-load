<template>
    <div class="container">
      <Product :items="products" @refetch="fetch">
        <template v-slot:item="{ item }">
           <h3>{{ item.title }}</h3>
           <p>{{ item.description }}</p>
        </template>
      </Product>
        
    </div>
</template>

<script>

import axios from 'axios'
import Product from './components/Product.vue';

export default {
  name: 'App',
  components: {
    Product
  },
  data() {
    return {
      products: [],
      lastPage:1
    }
  },
  methods: {
       async fetch(page) {
            if(page > this.lastPage) {
            return
          }

       let products = await axios.get(`http://localhost:8000/api/products/backend?page=${page}`)
       this.products.push(...products.data.data)
       this.lastPage = products.data.last_page
    },
  },
  mounted() {
    this.fetch(1)
  }
}
</script>


<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: 'Poppins', sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}

</style>
