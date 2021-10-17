<template>
	<div>
		<v-container>
			<v-btn color="error accent-3" text fab @click="dialog = true">
				<v-icon>mdi-folder-multiple-plus-outline</v-icon>	
			</v-btn>
			<v-row>
				<v-col cols="6" md="4" v-for="book in allBooks" :key="book.id">
					<v-card flat class="pa-5" color="#1b2a47" dark>
						<v-img height="200" :src="`http://127.0.0.1:8000${book.cover}`"></v-img>
						<div class="text-center pa-3 mt-5">
							<h2 class="mb-2">{{book.title}}</h2>
						</div>
						<div class="d-flex justify-space-between">
							<div>
								<v-btn icon color="orange" @click="editebtn = true, editeid = book"><v-icon>mdi-square-edit-outline</v-icon></v-btn>
								<v-btn icon color="error" @click="deletebtn = true, deleteid = book"><v-icon>mdi-trash-can-outline</v-icon></v-btn>
							</div>
							<a :href="`http://127.0.0.1:8000${book.book}`" class="text-decoration-none"><v-btn class="white--text" color="teal accent-3">DOWNLOAD</v-btn></a>
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
	       				<v-form @submit.prevent="add_book" enctype="multipart/form-data" v-model="valid">	       						
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
			       					<v-file-input @change="onFileSelectedImg" id="cover" placeholder="book image"/>
			       					<v-file-input @change="onFileSelectedPdf" id="file" placeholder="Only Pdf File!"/>
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
		            		@click="del_book(deleteid.id)"
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
	       				<v-form @submit.prevent="edit_book(editeid.id)" v-model="valid">	       						
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
			       					<v-file-input @change="onEditFileSelectedCover" id="editedCover" placeholder="Book Image"/>
			       					<v-file-input @change="onEditFileSelectedBook" id="editedBook" placeholder="Only Pdf File!"/>
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
				valid: true,
				editebtn: false,
				deletebtn: false,
				valid: true,
				dialog: false,
				allBooks: [],
				title: '',
				cover: null,
				book: null,
				editedTitle: '',
				editedCover: null,
				editedBook: null
			}
		},
		methods:{
			async get_books(){
				await this.$axios.get('http://127.0.0.1:8000/api/book-view/')
					.then(res => {
						this.allBooks = res.data
					})
			},
			async del_book(id){
				this.$axios.$delete(`http://127.0.0.1:8000/api/book-delete/${id}/`)
					.then(() => {
						this.allBooks = this.allBooks.filter(book => book.id !== id)
						this.deletebtn = false

					}).catch(err => {
						console.log(err)
					})
			},
			onFileSelectedImg(){
				this.cover = document.getElementById('cover').files[0]
			},
			onFileSelectedPdf(){
				this.book = document.getElementById('file').files[0]
			},
			async add_book(){
				const fd = new FormData()
				fd.append('title', this.title)
				fd.append('cover', this.cover, this.cover.name)
				fd.append('book', this.book, this.book.name)

				await this.$axios.post('http://127.0.0.1:8000/api/book-create/', fd)
					.then(res => {
						this.allBooks.push(res)
						this.get_books()
						this.dialog = false
						this.title = ''
						this.cover = null
						this.book =  null
					})

			},
			onEditFileSelectedCover(){
				this.editedCover = document.getElementById('editedCover').files[0]
			},
			onEditFileSelectedBook(){
				this.editedBook = document.getElementById('editedBook').files[0]
			},
			async edit_book(id){
				const edited = new FormData()
				edited.append('title', this.editedTitle)
				edited.append('cover', this.editedCover, this.editedCover.name)
				edited.append('book', this.editedBook, this.editedBook.name)

				this.$axios.patch(`http://127.0.0.1:8000/api/book-update/${id}/`, edited)
					.then(res => {
						this.allBooks = res.data
						this.get_books()
						this.editebtn = false
						this.editedTitle = ''
						this.editedCover = null
						this.editedBook =  null
					})
			}
		},
		created(){
			this.get_books()
		}
	}
</script>