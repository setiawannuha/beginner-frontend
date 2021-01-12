<template>
  <div class="home">
    <div class="container">
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
                  <Card v-bind:element="element"/>
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
              Body
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
export default {
  name: 'Home',
  components: {
    Card
  },
  data () {
    return {
      isLoading: true,
      listUsers: [],
      isError: false,
      errorMessage: ''
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
