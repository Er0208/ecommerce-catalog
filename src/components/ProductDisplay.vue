<template>
  <!-- v-if="product.category == 'men\'s clothing'  " -->
  <div v-if="product.category == 'men\'s clothing' ||product.category == 'women\'s clothing'">
      <div v-bind:class="product.category == 'men\'s clothing' ? 'men' : 'women'">
          <div class="container">
              <div class="images">
                  <!-- img from api -->
                  <img :src="product.image" alt="product image" />
              </div>
              <div class="product">
                  <h1>{{ product.title }}</h1>
                  <div class="justify">
                      <h6>{{ product.category }}</h6>
                      <h6>
                          {{ product.rating['rate'] }} / 5
                          <!-- if rating === 5 active class is 5 if 4 active is 4 -->
                          <span v-for="n in 5" :key="n" class="dot" :class="{ active: n <= product.rating['rate'] }">
                          </span>
                      </h6>
                  </div>
                  <hr />
                  <p class="desc">
                      {{ product.description }}
                  </p>
                  <hr />
                  <h2>${{ product.price }}</h2>
                  <button v-bind:class="product.category == 'men\'s clothing'? 'button-men': 'button'">
                      Add to Cart
                  </button>
                  <button-next v-bind:class="product.category == 'men\'s clothing'? 'button-next-men': 'button'" @click="nextProduct">Next Product</button-next>
              </div>
          </div>
      </div>
  </div>
  <div v-else>
      <!-- <div v-if="product.category !== 'women\'s clothing' && 'men\'s clothing'"> -->
      <div class="nodata">
          <div class="container">
              <div class="hero-image">
                  <div class="product">
                      <div class="fof">
                          <h1>This product is unavailable to show</h1>
                      </div>
                      <!-- <button class="button">Add to Cart</button> -->
                      <button-nodata @click="nextProduct">Next Product</button-nodata>
                  </div>
              </div>
          </div>
      </div>
  </div>
</template>

<script>
import axios from 'axios'
import { onMounted, ref } from 'vue'
export default {
  name: 'ProductDisplay',
  setup() {
      const product = ref({})
      onMounted(async () => {
          const response = await axios.get(
              'https://fakestoreapi.com/products/1'
          )
          product.value = response.data
      })

      //next product
      const nextProduct = async () => {
          //next id until id= 4
          // const category = product.value.category;
          const id = product.value.id
          // if nextid is 5 then nextid = 15 or if nextid is 20 then nextid = 1
          const rumus = () => {
              //if category !== women and men then go to id 15
              if (id === 5) {
                  return 15
              } else if (id === 20) {
                  return 1
              } else {
                  return id + 1
              }

              // if (category !==  'men\'s clothing' && category !== 'women\'s clothing' )
              // {
              //     //return to id >= 15
              //     return product.value.category = 15;
              // }
              // else if (category === 'women\'s clothing' && id ===20)
              // {
              //     return  product.value.id = 1;
              // }
              // else
              // {
              //     return id + 1;
              // }
          }
          // const nextId = id === 5 ? 15  : id + 1;
          const nextId = rumus()
          const response = await axios.get(
              `https://fakestoreapi.com/products/${nextId}`
          )
          product.value = response.data
      }

  
      return { product, nextProduct }
  },

  
  }
</script>

<style lang="css" scoped>
.men {
  width: 100%;
  display: grid;
  background-image: linear-gradient(#d6e6ff 70%, white 30%);
  
}

.women {
  width: 100%;
  display: grid;
  background-image: linear-gradient(#fde2ff 70%, white 30%);

}

.nodata {
  width: 100%;
  display: grid;
  background-image: linear-gradient(#dcdcdc 70%, white 30%);
  font-family: sans-serif;
  text-transform: uppercase;
}

.dot {
  height: 15px;
  width: 15px;
  background-color: #bbb;
  border-radius: 50%;
  display: inline-block;
  margin-right: 5px;
}

.active {
  background-color: #720060;
}

@import '../assets/style/page.css';
</style>
