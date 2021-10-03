<template>
    <div class="container">
        <div class="task" :key="product.id" v-for="product in products">
           <h3>{{ product.title }}</h3>
           <p>{{ product.description }}</p>
        </div>
          <div v-if="products.length" v-observe-visibility="handleScrolledToBottom"></div>
    </div>
</template>

<script>

import axios from 'axios';

export default {
  name: 'App',
  components: {

  },
  data() {
    return {
     products: [],
     page:1,
     lastPage:1
    }
  },
  methods: {
    async fetch() {
       let products = await axios.get(`http://localhost:8000/api/products/backend?page=${this.page}`)
       // this.products = products.data.data
       this.products.push(...products.data.data)
       this.lastPage = products.data.last_page
    },
      handleScrolledToBottom(isVisible) {
          if(!isVisible) {
              return
          }
          if(this.page >= this.lastPage) {
            return
          }
          this.page++;
          this.fetch()
      }
  },
  mounted() {
    this.fetch()
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
