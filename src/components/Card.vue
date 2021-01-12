<template>
  <div>
    <div class="card">
      <div class="card-body">
        {{element.id}} {{element.name}}
        <router-link :to="'/detail/' + element.id">Detail</router-link>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: ['element']
}
</script>
