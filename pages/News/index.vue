<template>
	<div>
		<v-container>
			<v-btn color="error accent-3" text fab @click="dialog = true">
				<v-icon>mdi-folder-multiple-plus-outline</v-icon>	
			</v-btn>
			<v-row>
				<v-col cols="12" md="4" sm="6" v-for="news in allNews" :key="news.id">
					<v-card flat color="#1b2a47" dark class="pb-10" width="300">
						<v-img width="300" :src="`http://127.0.0.1:8000${news.image}`"></v-img>
						<div class="text-center mt-4">
							<h2>{{news.title}}</h2>
						</div>
						<div class="text-center pa-3 mt-5">
						<v-expansion-panels dark focusable>
							<v-expansion-panel fluid>
						      <v-expansion-panel-header color="#1b2a47">
						        Full Desribtion
						      </v-expansion-panel-header>
						      <v-expansion-panel-content color="#1b2a47">
						        {{news.text}}
						      </v-expansion-panel-content>
						    </v-expansion-panel>
						</v-expansion-panels>
						</div>
						<div class="d-flex justify-space-between">
							<v-btn icon color="orange" class="ma-2" @click="editebtn = true, editeid = news"><v-icon>mdi-square-edit-outline</v-icon></v-btn>
							<v-btn icon color='error' class="ma-2" @click="deletebtn = true, deleteid = news"><v-icon>mdi-trash-can-outline</v-icon></v-btn>
						</div>
					</v-card>
				</v-col>
			</v-row>
			<v-row justify="center">
   				 <v-dialog
      				v-model="dialog"
      				max-width="600"

    			 >
      				<v-card color="#1b2a47" class="pa-10 text-center" dark>
	       				<v-form @submit.prevent="add_news" enctype="multipart/form-data" v-model="valid">	       						
		       				<v-card-text>	
								<h3>Create News</h3>
		       				</v-card-text>
		       				<v-card-text>
			       					<v-text-field
			       						outlined
			       						type="text"
			       						label="Title"
			       						color="teal accent-3"
			       						prepend-inner-icon="mdi-format-text"
			       						v-model="title"
			       					/>
								    <v-textarea
								      name="text"
								      filled
								      label="Text"
								      auto-grow
								      v-model="text"
								    ></v-textarea>
			       					<v-file-input @change="onFileSelected" id="file" />
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
      				v-model="deletebtn"
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
				            @click="deletebtn = false"
	         			>
	           			 Cansel
	          			</v-btn>
	          			<v-btn
		           			 color="error darken-1"
		            		@click="del_news(deleteid.id)"
	          			>
	            		  Delete
	         		 	</v-btn>
	        			</v-card-actions>
	      			</v-card>
    			</v-dialog>
  			</v-row>
  			<v-row justify="center">
   				 <v-dialog
      				v-model="editebtn"
      				max-width="600"

    			 >
      				<v-card color="#1b2a47" class="pa-10 text-center" dark>
	       				<v-form @submit.prevent="edit_news(editeid.id)" v-model="valid">	       						
		       				<v-card-text>	
								<h3>Edit Book</h3>
		       				</v-card-text>
		       				<v-card-text class="text-align-start">
			       					<v-text-field
			       						outlined
			       						type="text"
			       						label="Title"
			       						color="teal accent-3"
			       						prepend-inner-icon="mdi-format-text"
			       						v-model="editedTitle"
			       					/>
			       					<v-text-field 
			       						outlined
			       						type="text"
			       						label="Text"
			       						prepend-inner-icon="mdi-card-text-outline"
			       						color="teal accent-3"
			       						v-model="editedText"
			       					/>
			       					<v-file-input @change="onEditFileSelected" id="editedfile" />
		       				</v-card-text>
		        			<v-card-actions class="margin">
		          			<v-spacer></v-spacer>

		          			<v-btn
					            color="grey darken-1"
					            text
					            @click="editebtn = false"
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
				editebtn: false,
				deletebtn: false,
				valid: true,
				dialog: false,
				allNews:[],
				title: '',
				image: null,
				text: '',
				editedTitle: '',
				editedImag: null,
				editedText: ''
			}
		},
		methods:{
			async getNews(){
				await this.$axios.get('http://127.0.0.1:8000/api/site-view/')
					.then(res => {
						this.allNews = res.data
					})
			},
			onFileSelected(){
				this.image = document.getElementById('file').files[0]
			},
			async add_news(){
				const fd = new FormData()
				fd.append('title', this.title)
				fd.append('image', this.image, this.image.name)
				fd.append('text', this.text)
				await this.$axios.post('http://127.0.0.1:8000/api/site-news/', fd)
					.then(res => {
						console.log(res)
						this.allNews.push(res)
						this.getNews()
						this.dialog = false
						this.title =  ''
						this.text = ''
					})
					.catch(err => {
						console.log(err)
					})
			},
			async del_news(id){
				this.$axios.$delete(`http://127.0.0.1:8000/api/news-delete/${id}/`)
					.then(() => {
						this.allNews = this.allNews.filter(news => news.id !== id)
						this.deletebtn = false

					}).catch(err => {
						console.log(err)
					})
			},
			onEditFileSelected(){
				this.editedImg = document.getElementById('editedfile').files[0]
			},
			async edit_news(id){
				const edited = new FormData();
				edited.append('title', this.editedTitle)
				edited.append('image', this.editedImg)
				edited.append('text', this.editedText)

				await this.$axios.$patch(`http://127.0.0.1:8000/api/news-patch/${id}/`, edited)
					.then(res =>{
						this.allNews = res.data
						this.getNews()
						this.editebtn = false
					})

			}

		},
		created(){
			this.getNews()
		}
	}
</script>