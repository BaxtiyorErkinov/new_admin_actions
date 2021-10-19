<template>
  <v-app id="error">
    <v-container class="d-flex justify-center align-content-center cont">
      <v-card color="#1b2a47" class="pa-5 red--text text-center" width="600">
        <v-card-content>
          <h1 v-if="error.statusCode === 404">
            {{ pageNotFound }}
          </h1>
          <h1 v-else>
            {{ otherError }}
          </h1>
          <NuxtLink to="/admin/">
            Home
          </NuxtLink>
        </v-card-content>
      </v-card>
    </v-container>
  </v-app>
</template>

<script>
export default {
  layout: 'empty',
  props: {
    error: {
      type: Object,
      default: null
    }
  },
  data () {
    return {
      pageNotFound: 'Page Not Found',
      otherError: 'Check your internet connection or refresh this page :('
    }
  },
  head () {
    const title =
      this.error.statusCode === 404 ? this.pageNotFound : this.otherError
    return {
      title
    }
  },
  mounted(){
    if(this.error.statusCode === 404){
      this.$router.push('/admin/')
    }
  }
}
</script>

<style scoped>
  #error{
    background-color:#152036;
    color: red;
  }
h1 {
  font-size: 20px;
}
</style>
