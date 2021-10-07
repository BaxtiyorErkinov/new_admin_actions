<template>
	<div>
		<v-container fluid class="my-5">

			<v-expansion-panels dark>
				<v-expansion-panel>
					<v-expansion-panel-header class="text-start" color="#1b2a47">
						<v-row>
							<v-col cols="1">
								<v-avatar size="45" color="#152036">
									<img src="@/assets/images/avatar.png">
								</v-avatar>
							</v-col>
							<v-col cols="8" class="align-self-center ml-6">
								<h3 class="white--text">{{stud.name}}  {{stud.surname}}</h3>
							</v-col>
						</v-row>
					</v-expansion-panel-header>
					<v-expansion-panel-content class="px-4 pt-4 white--text" color="#152036">
						<div class="font-weight-bold">Email : {{stud.email}}</div>
						<v-divider class="my-1"/>
						<h3>Country : {{stud.country}}</h3>
						<v-divider class="my-1"/>
						<h3>Region : {{stud.region}}</h3>
						<v-divider class="my-1"/>	
						<v-row>
							<v-col cols="11">
								<h3 class="d-flex">Center : {{center.title}}</h3>
							</v-col>
							<v-col cols="1" class="justify-content-end">							
								<v-btn 
									class="d-flex" 
									fab 
									fluid 
									text 
									small 
									color="error" 
									@click="dialog = true"
									>
									<v-icon>mdi-trash-can-outline</v-icon>
								</v-btn>
							</v-col>
						</v-row>					
					</v-expansion-panel-content>
				</v-expansion-panel>
			</v-expansion-panels>
			<v-row justify="center">
   				 <v-dialog
      				v-model="dialog"
      				max-width="400"
    			 >
      				<v-card color="#1b2a47" class="pt-3 text-center" dark>
	       				 <v-card-text>	
							<h3>Are you sure you want to delete?</h3>
	       				</v-card-text>

	        			<v-card-actions class="margin">
	          			<v-spacer></v-spacer>

	          			<v-btn
				            color="grey darken-1"
				            text
				            @click="dialog = false"
	         			>
	           			 Cansel
	          			</v-btn>

	          			<v-btn
		           			 color="error darken-1"
		            		@click="deleteStud"
	          			>
	            		Delete
	         		 	</v-btn>
	        			</v-card-actions>
	      			</v-card>
    			</v-dialog>
  			</v-row>
		</v-container>
	</div>
</template>

<script>
	export default{
		head() {
			return{

      			title: `Student - ${this.stud.name} ${this.stud.surname}`
			}
    	},
		data(){
			return{
				dialog: false,
				stud: []
			}
		},
		async asyncData({$axios, params}){
			const stud = await $axios.$get(`http://127.0.0.1:8000/api/student-detail/${params.id}`)
			const center = await $axios.$get(`http://127.0.0.1:8000/api/educenter-detail/${stud.education}`)
			return{
				stud: stud,
				center
			}
		},
		methods:{
			async deleteStud() {
			 await this.$axios.$delete(`http://127.0.0.1:8000/api/student-delete/${this.stud.id}/`)
			 	.then(() => {
			   		this.stud.splice(this.stud.id, 1)
			 		this.$router.push('/student')
			 	})
			 	.catch(() => {
			 		this.$router.push('/student')
			 	})
           	}
		},
	}
</script>

<style scoped>

</style>
 