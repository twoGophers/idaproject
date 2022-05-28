<template>
  <section class="container">
    <App/>
  </section>
</template>

<script>

export default {
  name : "app",
}

</script>

<style lang="scss" scoped>

body {
    font-family: 'Source Sans Pro', sans-serif;
}

@import url('../assets/style/Reset.scss');
@import url('../assets/style/Font.scss');

.container {
  max-width: 1440px;
  box-sizing: border-box;
  margin: 0 auto;
  background: #E5E5E5;
}
</style>