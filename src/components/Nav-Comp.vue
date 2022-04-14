<template>
    <div class="nav-bar">
      <h4 class="nav-bar-title">{{ navtitle }}</h4>
    </div>
</template>

<script>
export default {
  name: 'Nav-Comp',
  props: {
    navtitle: String,
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
