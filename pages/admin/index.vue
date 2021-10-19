<template>
 <v-container>
    <v-row>
      <v-col cols="12" md="3" sm="6">
        <v-card class="pa-5" color="#1b2a47" dark>
          <div class="title">Umumiy O'quvchilar :</div>
          <h2 class="title" style="color:#24caa1">{{studs}}</h2>
          <v-progress-linear
            class="mt-2"
            v-model="studs"
            color="teal accent-3"
            height="6"
            rounded
          > 
           </v-progress-linear>
        </v-card>
      </v-col>
      <v-col cols="12" md="3" sm="6">
        <v-card class="pa-5" color="#1b2a47" dark>
          <div class="title">Umumiy Markazlar :</div>
          <h2 class="title" style="color:#eb4b4b">{{centers}}</h2>
          <v-progress-linear
            class="mt-2"
            v-model="centers"
            color="#eb4b4b"
            height="6"
            rounded
          > 
           </v-progress-linear>
        </v-card>
      </v-col>
      <v-col cols="12" md="3" sm="6">
        <v-card class="pa-5" color="#1b2a47" dark>
          <div class="title">Umumiy Test Turlari :</div>
          <h2 class="title" style="color:#2eb7f3">{{questions}}</h2>
          <v-progress-linear
            class="mt-2"
            v-model="questions"
            color="#2eb7f3"
            height="6"
            rounded
          > 
           </v-progress-linear>
        </v-card>
      </v-col>
      <v-col cols="12" md="3" sm="6">
        <v-card class="pa-5" color="#1b2a47" dark>
          <div class="title">Umumiy Testlar soni :</div>
          <h2 class="title" style="color:#805bbe">{{langs}}</h2>
          <v-progress-linear
            class="mt-2"
            v-model="langs"
            color="#805bbe"
            height="6"
            rounded
          > 
           </v-progress-linear>
        </v-card>
      </v-col>
      <v-col cols="12" md="6">
        <v-card color="#1b2a47" class="pa-3 ma-1 text-center" v-for="names in filteredItems" :key="names.id">
          <div class="title" style="color:#24caa1;">Student : <span>{{names.name}}  {{names.surname}}</span></div>
        </v-card>
        <v-btn text color="#2eb7f3" @click="route">
          <v-icon class="mr-1">mdi-more</v-icon>
          <span>More students</span>
        </v-btn>
      </v-col>
      <v-col cols="12" md="6">
        <v-card color="#1b2a47" class="pa-3 ma-1 text-center" v-for="names in filteredCenters" :key="names.id">
          <div class="title" style="color:#eb4b4b;">Markaz : <span>{{names.title}}</span></div>
        </v-card>
        <v-btn text color="#2eb7f3" @click="route_sec">
          <v-icon class="mr-1">mdi-more</v-icon>
          <span>More centers</span>
        </v-btn>
      </v-col>
    </v-row>
 </v-container>
</template>

<script>
  export default{
    head: {
      title: 'Admin Panel - Home'
    },
    data(){
      return{
          centers:0,
          studs:0,
          questions:0,
          langs:0,
          students: 0,
          studNames: [],
          centerNames: []
      }
    },
    computed: {
      filteredItems(){
        return this.studNames = this.studNames.filter(s => s.id <= 10)
      },
      filteredCenters(){
        return this.centerNames = this.centerNames.filter(c => c.id <= 10)
      }
    },
    methods:{
      is_authorized() {
       this.$axios.get('https://actions.uz/api/get-user/',{
        headers: {
          Authorization:"Bearer" + this.$auth.$storage.getLocalStorage("token")
        }}
      ).catch(
          err => {
            if (err){
              this.$router.push("/admin/login")
            }
          }
        )
      
    },
      get_data(){
         this.$axios.get('https://actions.uz/api/get-full-data/')
        .then (res => {
          let data = res.data
          this.studs = data['students']
          this.centers = data['centers']
          this.questions = data['questions']
          this.langs = data['languages']
        }).catch(
          err => {
            console.log(err)
          }
        )  
      },
      get_studs(){
        this.$axios.get('https://actions.uz/api/student-view/')
          .then(res => {
            let data = res.data
            this.studNames = data
          })
      },
      get_centers(){
        this.$axios.get('https://actions.uz/api/educenter-view/')
          .then(res => {
            let data = res.data
            this.centerNames = data
          })
      },
      route(){
        this.$router.push('/admin/student')
      },
      route_sec(){
        this.$router.push('/admin/center')

      }
  },
    created(){
      this.get_centers()
      this.get_studs()
      this.get_data()
      // this.is_authorized()
    },
    mounted(){
      try{
       this.$axios.get('https://actions.uz/api/get-user/', {
          headers: {
               Authorization:"Bearer " + this.$auth.$storage.getLocalStorage("token")
            }
        })
      } catch{
        this.$router.push('/admin/Login')
      }
    },
}
</script>

<style>
  span{
    color: #fff;
  }
</style>
