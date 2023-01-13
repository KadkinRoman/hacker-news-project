<template>
  <div>
    <navbar-item />
    <main>
      <div class="container">
        <Nuxt />
      </div>
    </main>
  </div>
</template>

<script>
import NavbarItem from '../components/NavbarItem.vue'
  export default {
  components: { NavbarItem },

  }
</script>
