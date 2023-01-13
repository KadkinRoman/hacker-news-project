<template>
  <b-navbar toggleable="lg" type="light" variant="light">
    <b-navbar-brand to="/news">Hacker news</b-navbar-brand>

    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav>
        <b-nav-item to="/news">News</b-nav-item>
      </b-navbar-nav>
    </b-collapse>
  </b-navbar>
</template>

<script>
  export default {
    components: true
  }
</script>
