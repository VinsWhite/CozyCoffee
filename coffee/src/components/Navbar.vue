<script setup>
import { ref } from 'vue'

const changeColorNav = ref(true)

const toggleNavbarColor = () => {
  changeColorNav.value = !changeColorNav.value
}
</script>

<template>
  <div>
    <nav 
      class="navbar navbar-expand-lg fixed-top"
      :class="{'navbar-light bg-light': !changeColorNav, 'navbar-dark bg-dark': changeColorNav}"
    >
      <div class="container-fluid">
        <img src="/cozy-coffee.jfif" class="me-2 rounded-circle" alt="cozy coffee icon" id="icon">
        <router-link class="nav-brand fw-bold fs-3 text-light text-decoration-none me-2" to="/">Cozy Coffee</router-link>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <li class="nav-item">
                <router-link class="nav-link" to="/">Home</router-link>
            </li>
            <li>
                <router-link class="nav-link" to="/about">About</router-link>
            </li>
          </ul>
          <form @click.prevent="toggleNavbarColor">
            <button type="button" class="btn" :class="{'btn-light': changeColorNav, 'btn-dark': !changeColorNav}">Change Color</button>
          </form>
        </div>
      </div>
    </nav>
  </div>
</template>
