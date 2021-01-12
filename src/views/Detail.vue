<template>
  <div>
     <div class="container">
      <h1>Detail {{id}}</h1>
      <div v-if="isLoading">
        <h1>Loading</h1>
      </div>
      <div v-else>
        {{result.name}}
        {{result.price}}
      </div>
     </div>
  </div>
</template>
<script>
import Axios from 'axios'
export default {
  data () {
    return {
      id: this.$route.params.id,
      isLoading: true,
      result: {}
    }
  },
  methods: {
    getDetailData: function () {
      Axios.get('http://localhost:3000/book/' + this.id).then((response) => {
        console.log(response.data)
        this.result = response.data.data
      }).catch((err) => {
        console.log(err)
      }).finally(() => {
        this.isLoading = false
      })
    }
  },
  mounted () {
    this.getDetailData()
  }
}
</script>
