<template>
  <div id="app">
    <div id="nav">
      <ul class="nav justify-content-center">
        <li class="nav-item">
          <router-link to="/" class="nav-link">Home</router-link>
        </li>
        <li class="nav-item">
          <router-link to="/about" class="nav-link">About</router-link>
        </li>
      </ul>
    </div>
    <router-view/>
  </div>
</template>
