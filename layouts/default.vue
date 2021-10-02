<template>
  <v-app id="main"> 
    <NavBar />
    <v-main class="color mx-4 mb-4 darken-4">
      <Nuxt/>
    </v-main>
  </v-app>
</template>

<script>
export default {
 
}
</script>

<style scoped>
  #main{
    background-color: #152036;
    color: #fff;
  }
  .color{
    background-color: #152036;
  }
</style>
