<template>
  <div class="home">
    <div class="container">
      <Navbar title="Home" type="0"/>
      <button v-on:click="setSidemenu()">Icon Menu</button>
      <div class="mt-5 mb-5">
        <div v-if="sidemenuShow">
          <Sidemenu/>
        </div>
      </div>
      <div class="row">
        <div class="col-md-8">
          <div class="card">
            <div class="card-header">
              List Users
            </div>
            <div class="card-body">
              <div v-if="isLoading">
                <h1>Loading</h1>
              </div>
              <div v-else-if="isError">
                <h1>{{errorMessage}}</h1>
              </div>
              <div v-else>
                <div v-for="(element, index) in listUsers" :key="index">
                  <div class="m-2">
                    {{element.name}}
                    <button @click="addToCart(element)">Add to Cart</button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="col-md-4">
          <div class="card">
            <div class="card-header">
              Cart
            </div>
            <div class="card-body">
              <div v-for="(item, index) in dataCart" :key="index">
                {{item.name}} - {{item.price}}
                <br/>
                Qty: {{item.qty}}
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Axios from 'axios'
import Navbar from '../components/Navbar'
import Sidemenu from '../components/Sidemenu'
export default {
  name: 'Home',
  components: {
    Navbar,
    Sidemenu
  },
  data () {
    return {
      isLoading: true,
      listUsers: [],
      isError: false,
      errorMessage: '',
      sidemenuShow: false,
      dataCart: []
    }
  },
  methods: {
    setSidemenu: function () {
      this.sidemenuShow = !this.sidemenuShow
    },
    addToCart: function (value) {
      // check apakah produk sudah ada di cart
      const checkProduk = this.dataCart.filter((item) => {
        return item.id === value.id
      })
      if (checkProduk.length >= 1) {
        // apabila produk ada di cart
        // cari produk yg id nya sama kemudian update qty nya
        this.dataCart.forEach(element => {
          if (element.id === value.id) {
            element.qty += 1
          }
        })
      } else {
        // apabila produk tidak ada di cart
        const dataBaru = {
          id: value.id,
          qty: 1,
          name: value.name,
          price: value.price,
          img: value.img
        }
        this.dataCart = [...this.dataCart, dataBaru]
      }
    }
  },
  mounted () {
    setTimeout(() => {
      Axios.get('http://localhost:3000/books?limit=100').then((response) => {
        this.listUsers = response.data.data
      }).catch(() => {
        this.isError = true
        this.errorMessage = 'Error saat fetch API'
      }).finally(() => {
        this.isLoading = false
      })
    }, 2000)
  }
}
</script>
