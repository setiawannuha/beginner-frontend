<template>
  <div>
    <div class="card">
      <div class="card-body">
        <img src=""  alt="" srcset="">
        <img :src="element.img" style="height:100px;width:100px" alt="" srcset="">
        {{element.id}} {{element.name}}
        <router-link :to="'/detail/' + element.id">Detail</router-link>
        <button v-on:click="addToCart(element)">Add to cart</button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: ['element'],
  methods: {
    addToCart: function (element) {
      this.$emit('actionDariChild', element)
    }
  }
}
</script>
