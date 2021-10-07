<template>
	<div>
		<v-container>
			<v-card color="#152036">
				<v-list style="height: 400px; overflow-y: scroll;" color="#1b2a47" class="scrollbar">
				  <v-list-item-group color="primary">
				    <v-list-item v-for="stud in studNames" :key="stud.id" class="mb-2">
				      	<v-avatar size="40" class="mr-5">
				      		<img src="@/assets/images/avatar.png">
				      	</v-avatar>
				      <v-list-item-content class="white--text">
				        <v-list-item-title><a :href="`/student/${stud.id}`">{{ stud.name }} {{stud.surname}}</a></v-list-item-title>
				      </v-list-item-content>
				      <v-list-item-action>
				      	<v-btn fab color="error" text @click="dialog = true">
				      		<v-icon>mdi-trash-can-outline</v-icon>
				      	</v-btn>
				      </v-list-item-action>
				    </v-list-item>
				  </v-list-item-group>
				</v-list>
			</v-card>
			<v-row justify="center" v-for="stud in studNames" :key="stud.id">
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
		            		@click.prevent="deleteStud(stud)"
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
		head: {
      		title: 'Admin Panel - Students'
    	},
		data(){
			return{
				studNames: [],
				dialog: false,
			}
		},
		created(){
			this.get_studs()
		},
		methods:{
			get_studs(){
	       		this.$axios.get('http://127.0.0.1:8000/api/student-view/')
	          		.then(res => {
	            		let data = res.data
	            		this.studNames = data
	          		})
	      	},
			async deleteStud(stud) {
			   await this.$axios.$delete(`http://127.0.0.1:8000/api/student-delete/${stud.id}/`)
			   	.then(() => {
			   		this.studNames.splice(stud, 1)
			   		this.dialog = false
			   	})
			   	.catch(() => {
			   		this.$nuxt.refresh()
			   	})
           	}
		},
	}
</script>

<style scoped>
	.scrollbar::-webkit-scrollbar{
		width: 20px;
	}
	.scrollbar::-webkit-scrollbar-track {
    	background-color: #152036;
    	border-radius: 10px;
	}
	.scrollbar::-webkit-scrollbar-thumb {
    	background-color: #1b2a47;
    	border-radius: 10px;
	}
	a{
		text-decoration: none;
		color: #fff;
	}
</style>