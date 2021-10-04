<template>
	<div>
		<v-container>
			<v-btn color="orange accent-3" text fab @click="dialog = true">
				<v-icon>mdi-folder-multiple-plus-outline</v-icon>	
			</v-btn>
			<v-card color="#152036">
				<v-list style="height: 400px; overflow-y: scroll;" color="#1b2a47" class="scrollbar">
				  <v-list-item-group color="primary" v-for="lang in allLanguages" :key="lang.id">
				    <v-list-item class="mb-2" dark>
				    	<v-icon color="teal accent-3">mdi-globe-model</v-icon>
				      <v-list-item-content class="white--text ml-4 title">
				        <v-list-item-title>
				        	<span class="teal--text text--accent-3"
				        	>
				        	Language
				        	</span>
				        	 : {{lang.title}} ?
				        	</v-list-item-title>
				      </v-list-item-content>
				      <v-list-item-action>
				      	<div>
					      	<v-btn fab color="warning" text @click="editor = true,langid = lang">
					      		<v-icon>mdi-square-edit-outline</v-icon>
					      	</v-btn>
					      	<v-btn fab text color="error" @click="modal = true, langid = lang">
					      		<v-icon>mdi-trash-can-outline</v-icon>
					      	</v-btn>
				      	</div>
				      </v-list-item-action>
				    </v-list-item>
				  </v-list-item-group>
				</v-list>
			</v-card>
			<v-row justify="center">
   				 <v-dialog
      				v-model="dialog"
      				max-width="600"

    			 >
      				<v-card color="#1b2a47" class="pa-10 text-center" dark>
	       				<v-form @submit.prevent="add_lang" v-model="valid">	       						
		       				<v-card-text>	
								<h3>Adding new Language</h3>
		       				</v-card-text>
		       				<v-card-text>
			       					<v-text-field
			       						outlined
			       						type="text"
			       						label="Question"
			       						color="teal accent-3"
			       						prepend-icon="mdi-format-text"
			       						v-model="form.title"
			       						:rules="rules"
			       					/>
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
			           			 color="teal accent-3"
			            		type="submit"
			            		:disabled="!valid"
		          			>
		            		Add
		         		 	</v-btn>
		        			</v-card-actions>
	       				</v-form>
	      			</v-card>
    			</v-dialog>
  			</v-row>
  			<v-row justify="center">
   				 <v-dialog
      				v-model="modal"
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
				            @click="modal = false"
	         			>
	           			 Cansel
	          			</v-btn>

	          			<v-btn
		           			 color="error darken-1"
		            		@click="deleteLang(langid.id)"
	          			>
	            		  Delete
	         		 	</v-btn>
	        			</v-card-actions>
	      			</v-card>
    			</v-dialog>
  			</v-row>
			<v-row justify="center">
   				 <v-dialog
      				v-model="editor"
      				max-width="600"

    			 >
      				<v-card color="#1b2a47" class="pa-10 text-center" dark>
	       				<v-form @submit.prevent="editLang(langid.id)" v-model="valid">	       						
		       				<v-card-text>	
								<h3>Edit Language</h3>

		       				</v-card-text>
		       				<v-card-text>
			       					<v-text-field
			       						outlined
			       						type="text"
			       						label="Language"
			       						color="teal accent-3"
			       						prepend-icon="mdi-format-text"
			       						v-model="edited.title"
			       						:rules="rules"
			       					/>
		       				</v-card-text>
		        			<v-card-actions class="margin">
		          			<v-spacer></v-spacer>

		          			<v-btn
					            color="grey darken-1"
					            text
					            @click="editor = false"
		         			>
		           			 Cansel
		          			</v-btn>

		          			<v-btn
			           			 color="teal accent-3"
			            		type="submit"
			            		:disabled="!valid"
		          			>
		            		Edit
		         		 	</v-btn>
		        			</v-card-actions>
	       				</v-form>
	      			</v-card>
    			</v-dialog>
  			</v-row>
		</v-container>
	</div>
</template>

<script>
	export default{
		data(){
			return{
				rules: [
        			value => !!value || 'This field is required.',
        			value => (value && value.length >= 5) || 'Min 5 characters',
      			],
      			valid: true,
				allLanguages: [],
				dialog: false,
				modal: false,
				langid: null,
				editor: false,
				form: {
					title: '',
					time: new Date()
				},
				edited: {
					title: '',
					time: new Date()
				}
			}
		},
		created(){
			this.get_langs()
		},
		methods: {
			get_langs(){
				this.$axios.get('http://127.0.0.1:8000/api/language-view/')
				 .then(res => {
						let data = res.data
						this.allLanguages = data
					})
			},
			async add_lang(){
				this.$axios.$post('http://127.0.0.1:8000/api/language-create/', this.form)
					.then(result => {
						this.allLanguages.push(result)
						this.dialog = false
						this.form.title = ''
					})
			},
			deleteLang(id){
				console.log(id)
				this.$axios.$delete(`http://127.0.0.1:8000/api/language-delete/${id}/` )
					.then(() => {
						this.allLanguages = this.allLanguages.filter(lang => lang.id !== id)
						this.modal = false
					})
					.catch(result => {
			   			console.log(result)
			   		})	
			},
			async editLang(lang){
				await this.$axios.$post(`http://127.0.0.1:8000/api/language-update/${lang}/`, this.edited)
					.then(res =>{
						this.allLanguages = res.data
						this.get_langs()
						this.editor = false
					})
			}
		},
	}
</script>