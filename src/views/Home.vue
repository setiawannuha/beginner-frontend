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
                  <Card v-bind:element="element" v-on:actionDariChild="eventChild"/>
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
              <div v-for="(items, index) in dataCart" :key="index">
                <img :src="items.img" style="width:50px;height:50px;" alt="" srcset="">
                {{items.name}}
                <br/>
                Qty: {{items.qty}}
                <br/>
                <button @click="deleteCart(items.id)">Tidak jadi</button>
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
import Card from '../components/Card'
import Navbar from '../components/Navbar'
import Sidemenu from '../components/Sidemenu'
export default {
  name: 'Home',
  components: {
    Card,
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
    deleteCart: function (id) {
      const dataCartBaru = this.dataCart.filter((item) => {
        return item.id !== id
      })
      this.dataCart = dataCartBaru
    },
    eventChild: function (element) {
      // alert('123')
      const checkCart = this.dataCart.filter((item) => {
        if (item.id === element.id) {
          return item
        }
      })
      if (checkCart.length >= 1) {
        this.dataCart.forEach(item => {
          if (item.id === element.id) {
            item.qty += 1
          }
        })
      } else {
        const dataBaru = {
          id: element.id,
          name: element.name,
          price: element.price,
          img: element.img,
          qty: 1
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
