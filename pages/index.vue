<template>
  <div class="app">   
    <main>
      <search-input v-model="searchKeyword" @search="searchProducts"></search-input>
      <ul>
        <li class="item flex" v-for="product in products" :key="product.id" @click="moveToDetailPage(product.id)">          
          <img class="product-image" :src="product.imageUrl" :alt="product.name">
          <p>{{product.name}}</p>
          <span>{{product.price}}</span>
        </li>
      </ul>
      <div class="cart-wrapper">
        <button class="btn" @click="moveToCartPage">장바구니 바로가기</button>
      </div>
    </main>    
  </div>  
</template>

<script>
import SearchInput from '@/components/SearchInput.vue';
import { fetchProductByKeyword, fetchProducts } from '~/api';

export default {
  components: { SearchInput },  
  async asyncData(){
    const response = await fetchProducts()    
    const products = response.data.map((item) => {
      return{
        ...item,
      imageUrl: `${item.imageUrl}?random=${Math.random()}`  
      }      
    })
    return { products }
  },

  data(){
    return{
      searchKeyword: '',
    }
  },

  methods: {
    moveToDetailPage(id){
      console.log(id);
      this.$router.push(`detail/${id}`)
    },    

    async searchProducts(){
      const response = await fetchProductByKeyword(this.searchKeyword)
      this.products = response.data.map((item) => ({
        ...item,
        imageUrl: `${item.imageUrl}?random=${Math.random()}`  
      }))
    },

    moveToCartPage(){
      this.$router.push('/cart');
    },
  }
}
</script>

<style scoped>
  .flex {
    display: flex;
    justify-content: center;
  }
  .item {
    display: inline-block;
    width: 400px;
    height: 300px;
    text-align: center;
    margin: 0 0.5rem;
    cursor: pointer;
  }
  .product-image {
    width: 400px;
    height: 250px;
  }
  .app {
    position: relative;
  }
  .cart-wrapper {
    position: sticky;
    float: right;
    bottom: 50px;
    right: 50px;
  }
  .cart-wrapper .btn {
    display: inline-block;
    height: 40px;
    font-size: 1rem;
    font-weight: 500;
  }
  </style>