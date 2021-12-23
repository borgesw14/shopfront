<template>
<section class="section">
  <div class="container">
    <div class="section">
      <h1 class="title">Cart</h1>
      <div v-for="item in cart" 
        :key="item.id">
        <nav class="level ">
          <div class="level-left has-text-centered">
            <p class="title">{{item.pokemon.name}}</p>
          </div>
          <div class="level-right has-text-right">
            <div class="level-item">
              <p class="title">Quantity: {{item.quantity}}</p>
            <button class="button is-info ml-6 my-2" @click="removeFromCart(item.id)">Remove</button>
            </div>
          </div>

        </nav>
      </div>
    </div>
    
    
    <h1 class="title">Pokemon for Sale</h1>
    <div class="columns is-multiline">
      <div class="column is-one-third" v-for="item in inventory" :key="item.id">
        <div class="card">
          <div class="card-image">
            <figure class="image is-square">
              <img :src="item.pictureUrl" alt="Pokemon image">
            </figure>
          </div>
          <div class="card-content">
            <h1 class="title">{{item.name}}</h1>
            <h2 class="title is-4">Price: {{item.price}}</h2>
          </div>
          <div class="card">
            <footer class="card-footer">
              <div class="card-footer-item">
                 <div class="field has-addons">
                  <div class="control">
                    <input class="input" type="number" placeholder="Quantity" v-model="quantity">
                  </div>
                  <div class="control">
                    <button class="button is-info" @click="addToCart(item.id)">Add to Cart</button>
                  </div>
                </div>
              </div>
            </footer>
          </div>
         </div>
        </div>
      </div>
    </div>
  
</section>
  
</template>

<script>
import axios from 'axios';
export default {
  
  name: "Home",
  components: {
  },
  data () {
    return{
      inventory: null,
      cart: null,
      quantity: null
    }
  },
  mounted (){
    axios.get("http://localhost:8080/api").then(response => (this.inventory = response.data))
    axios.get("http://localhost:8080/api/cart").then(response => (this.cart = response.data))
  },
  methods: {
    async addToCart(item) {
      let result = await axios.post("http://localhost:8080/api/addtocart/"+ item + "/" + this.quantity);
      console.log(result.response)
      document.location.reload(true)
    },
    async removeFromCart(item) {
      let result = await axios.delete("http://localhost:8080/api/deleteItem/" + item)
      console.log(result.response)
      document.location.reload(true)
    }
  }
};
</script>
