<template>
  <v-app id="inspire">
        <v-row align="center" justify="center" class="mx-1">
          <v-col cols="12" sm="8" md="6">
            <v-card class="elevation-12 border" color="#1b2a47" width="650" height="310">
                  <v-row class="mb-3">
                    <v-col cols="12" md="10" class="center">
                      <v-card-text class="mt-5">
                        <h1
                          class="text-center display-2 teal--text text--accent-3"
                        >Sign in</h1>
                        <v-form @submit.prevent="loginForm" ref="form" v-model="valid">
                          <v-text-field
                          	dark
                            label="Username"
                            name="username"
                            prepend-icon="mdi-account"
                            type="text"
                            color="teal accent-3"
                            v-model="user.username"
                            :rules="inputRules"
                            :counter="5"
                          />

                          <v-text-field
                          	dark
                            id="password"
                            label="Password"
                            name="password"
                            prepend-icon="mdi-lock"
                            type="password"
                            color="teal accent-3"
                            v-model="user.password"
                            :rules="inputRules"
                            :counter="5"
                          />
                      <div class="text-center mt-3">
                        <v-btn 
                        	rounded 
                        	color="teal accent-3" 
                        	type="submit" 
                        	dark 
                        	:disabled="!valid"
                        	 @click="validate"
                        	>
                        	SIGN IN
                    	</v-btn>
                      </div>
                        </v-form>
                      </v-card-text>
                    </v-col>
                  </v-row>
            </v-card>
                <v-alert
                  width="650"
                  class="mt-3"
			      			prominent
			      			type="error"
			      			v-model="alert"
			      			transition="slide-y-transition"
			    			>
			      <v-row align="center">
			        <v-col class="grow text-center">
								Username or password wrong !!!
			        </v-col>
			        <v-col class="shrink">
			          <v-btn @click="alert = false" text><v-icon>mdi-close</v-icon></v-btn>
			        </v-col>
			      </v-row>
			    </v-alert>
          </v-col>
        </v-row>
  </v-app>
</template>

<script>
export default {
	data(){
		return{
			user: {
				username: '',
				password: ''
			},
			alert: false,
			inputRules: [
				v => v.length >= 5 || 'Maximum 5 characters'
			],
			valid: true
			

		}
	},
    methods:{
    	 async loginForm(){
	      	await this.$axios.post("https://actions.uz/api/token/", this.user)

		        .then(res => {
		          this.$auth.$storage.setLocalStorage("token", res.data.access)
		          this.$router.push("/admin/")
		        })
		        .catch((err) => {
		          if (err) {
		            this.alert = true;
		          }
		        });
	    	},
	   	validate () {
        	this.$refs.form.validate()
      	},
    }
}
</script>

<style scoped>
	#inspire{
		background-color: #152036;
	}
	.flex{
		margin: auto;
	}
	.pos{
		display: flex;
		justify-content: center;
	}
	.border{
		border-radius: 5px;
	}
	.rgb{
		background-color: rgb(104, 255, 74);
	}
	.center{
		margin:0 auto ;
	}
	.full{
		z-index: 1000;
		height: 70vh;
	}
	input{
		color: white;
	}
</style>