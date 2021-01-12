<template>
  <div class="about">
    <div class="container">
      <h1>Insert Book</h1>
      <div class="card">
        <div class="card-body">
          <input type="text" v-model="formData.nama" class="form-control mb-2" placeholder="Name">
          <input type="text" v-model="formData.harga" class="form-control mb-2" placeholder="Price">
          <input type="text" v-model="formData.deskripsi" class="form-control mb-2" placeholder="Description">
        </div>
        <div class="card-footer">
          <button class="btn btn-success" @click="onInsert">Simpan</button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import Axios from 'axios'
import router from '../router/index'
export default {
  data () {
    return {
      formData: {
        nama: '',
        harga: '',
        deskripsi: ''
      }
    }
  },
  methods: {
    onInsert: function () {
      Axios.post('http://localhost:3000/books', this.formData).then((response) => {
        console.log(response)
        router.push({ path: '/' })
      }).catch((err) => {
        console.log(err)
      })
    }
  }
}
</script>
