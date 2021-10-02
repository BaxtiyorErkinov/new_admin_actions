<template>
	<div>
		<v-container>
			<v-btn color="error accent-3" text fab @click="dialog = true">
				<v-icon>mdi-folder-multiple-plus-outline</v-icon>	
			</v-btn>
			<v-row justify="center">
				<v-col cols="12" sm="6" md="4" lg="3" v-for="(certificat, index) in allCertificates" :key="certificat.id">
					<v-card flat class="text-center cursor py-5" color="#1b2a47" dark>
						<v-btn fab fluid class="index" color="#F4B400" small><h4>{{index+1}}</h4></v-btn>
						<v-responsive class="text-h5">
							<v-avatar class="d-block ma-auto" size="100" tile>
								<img src="@/assets/images/noimg.jpg" v-if="certificat.image == null">
								<img v-else class="img" :src="`http://127.0.0.1:8000${certificat.image}`">
							</v-avatar>
							<div class="mt-4">							
								<v-icon large color="error">mdi-certificate-outline</v-icon>
								<span>Certificate</span>
							</div>
						</v-responsive>
						<v-responsive class="pt-4">
						</v-responsive>
						<v-card-subtitle>
							<div class="text-h6">
								<span class="hovered">{{ certificat.title }}</span>
							</div>
							<v-divider  class="my-1"/>
							<div class="title">
								<span class="hovered">{{ certificat.text }}</span>
							</div>
						</v-card-subtitle>
						<v-card-text class="d-flex justify-space-between">
							<v-btn fab text color="warning" class="ma-0" @click="editebtn = true, editeid = certificat">
								<v-icon>mdi-square-edit-outline</v-icon>
							</v-btn>							
							<v-btn fab text color="error"  class="ma-0" @click="deletebtn = true, deleteid = certificat">
								<v-icon>mdi-trash-can-outline</v-icon>
							</v-btn>
						</v-card-text>
					</v-card>
				</v-col>
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
		            		@click="del_certificate(deleteid.id)"
	          			>
	            		  Delete
	         		 	</v-btn>
	        			</v-card-actions>
	      			</v-card>
    			</v-dialog>
  			</v-row>
			<v-row justify="center">
   				 <v-dialog
      				v-model="dialog"
      				max-width="600"

    			 >
      				<v-card color="#1b2a47" class="pa-10 text-center" dark>
	       				<v-form @submit.prevent="add_certificate" enctype="multipart/form-data" v-model="valid">	       						
		       				<v-card-text>	
								<h3>Create Certificate</h3>
		       				</v-card-text>
		       				<v-card-text>
			       					<v-text-field
			       						outlined
			       						type="text"
			       						label="Title"
			       						color="teal accent-3"
			       						prepend-inner-icon="mdi-format-text"
			       						v-model="title"
			       						:rules="rules"
			       					/>
			       					<v-text-field 
			       						outlined
			       						type="text"
			       						label="Text"
			       						prepend-inner-icon="mdi-card-text-outline"
			       						color="teal accent-3"
			       						v-model="text"
			       						:rules="rules"
			       					/>
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
      				v-model="editebtn"
      				max-width="600"

    			 >
      				<v-card color="#1b2a47" class="pa-10 text-center" dark>
	       				<v-form @submit.prevent="edit_certificate(editeid.id)" v-model="valid">	       						
		       				<v-card-text>	
								<h3>Edit Certificate</h3>
		       				</v-card-text>
		       				<v-card-text class="text-align-start">
			       					<v-text-field
			       						outlined
			       						type="text"
			       						label="Title"
			       						color="teal accent-3"
			       						prepend-inner-icon="mdi-format-text"
			       						v-model="editedTitle"
			       						:rules="rules"
			       					/>
			       					<div>
			       						
			       					<p class="mt-0" align="start">Old Value:</p>
			       					</div>
			       					<v-text-field 
			       						outlined
			       						type="text"
			       						label="Text"
			       						prepend-inner-icon="mdi-card-text-outline"
			       						color="teal accent-3"
			       						v-model="editedText"
			       						:rules="rules"
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
				rules: [
        			value => !!value || 'This field is required.',
        			value => (value && value.length >= 5) || 'Min 5 characters',
      			],
      			valid: true,
				allCertificates: [],
				dialog: false,
				deletebtn: false,
				deleteid: null,
				editebtn: false,
				editeid: null,
				title: '',
				image: null,
				text: '',
				editedTitle: '',
				editedImg: null,
				editedText: ''
			}
		},
		methods:{
			get_certificate(){
				this.$axios.get('http://127.0.0.1:8000/api/sertificate-get/')
					.then(res => {
						let data = res.data
						this.allCertificates = data
					})
			},
			onFileSelected(){
				this.image = document.getElementById('file').files[0]
				console.log(this.image)
			},
			async add_certificate(){
				const fd = new FormData()
				fd.append('title', this.title)
				fd.append('image', this.image, this.image.name)
				fd.append('text', this.text)
				await this.$axios.post('http://127.0.0.1:8000/api/sertificate-create/', fd)
					.then(res => {
						console.log(res)
						this.allCertificates.push(res),
						this.get_certificate()
						this.dialog = false
					})
					.catch(err => {
						console.log(err)
					})
			},
			async del_certificate(id){
				this.$axios.$delete(`http://127.0.0.1:8000/api/sertificate-delete/${id}/`)
					.then(() => {
						this.allCertificates = this.allCertificates.filter(cert => cert.id !== id)
						this.deletebtn = false
					})
					.catch(result => {
			   			console.log(result)
			   		})	
			},
			onEditFileSelected(){
				this.editedImg = document.getElementById('editedfile').files[0]
				console.log(this.editedImg)
			},
			async edit_certificate(id){
				const edited = new FormData();
				edited.append('title', this.editedTitle)
				edited.append('image', this.editedImg)
				edited.append('text', this.editedText)

				await this.$axios.$patch(`http://127.0.0.1:8000/api/sertificate-patch/${id}/`, edited)
					.then(res =>{
						this.allCertificates = res.data
						this.get_certificate()
						this.editebtn = false
						console.log(res)
					})
			},
		},
		created(){
			this.get_certificate()
		}
	}
</script>

<style scoped>
	*{
		box-sizing: border-box;
	}
	.index{
		position: absolute;
		top: -10px;
		left: -10px;
	}
	.hovered{
		transition: all .5s;
	}
	.hovered:hover{
		color: #fff;
		cursor: pointer;
	}
	.img{
		border-radius: 5px;
	}
</style>