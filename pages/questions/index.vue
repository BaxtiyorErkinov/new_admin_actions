<template>
	<div>
		<v-container>
			<v-btn color="teal accent-3" text fab @click="dialog = true">
				<v-icon>mdi-folder-multiple-plus-outline</v-icon>	
			</v-btn>
			<v-card color="#152036">
				<v-list style="height: 400px; overflow-y: scroll;" color="#1b2a47" class="scrollbar">
				  <v-list-item-group color="primary" v-for="question in allQuestions" :key="question.id">
				    <v-list-item class="mb-2" dark>
				    	<v-icon color="teal accent-3">mdi-comment-question</v-icon>
				      <v-list-item-content class="white--text ml-4 title">
				        <v-list-item-title>
				        	<span class="teal--text text--accent-3"
				        	>
				        	Question
				        	</span>
				        	 : {{question.qestion}} ?
				        	</v-list-item-title>
				      </v-list-item-content>
				      <v-list-item-action>
				      	<v-btn fab color="warning" text @click="editor = true, questid = question">
				      		<v-icon>mdi-square-edit-outline</v-icon>
				      	</v-btn>
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
	       				<v-form @submit.prevent="postQuest" v-model="valid">	       						
		       				<v-card-text>	
								<h3>Adding Question</h3>
		       				</v-card-text>
		       				<v-card-text>
			       					<v-text-field
			       						outlined
			       						type="text"
			       						label="Question"
			       						color="teal accent-3"
			       						prepend-icon="mdi-format-text"
			       						v-model="form.qestion"
			       						:rules="rules"
			       					/>
			       					<v-text-field 
			       						outlined
			       						type="text"
			       						label="Answer"
			       						color="teal accent-3"
			       						prepend-icon="mdi-ab-testing"
			       						v-model="form.answer"
			       						:rules="rules"
			       					/>
			       					<v-text-field 
			       						outlined
			       						type="text"
			       						label="A - variant"
			       						color="teal accent-3"
			       						prepend-icon="mdi-alphabetical-variant"
			       						v-model="form.a"
			       						:rules="rules"
			       					/>
			       					<v-text-field 
			       						outlined
			       						type="text"
			       						label="B - variant"
			       						color="teal accent-3"
			       						prepend-icon="mdi-alphabetical-variant"
			       						v-model="form.b"
			       						:rules="rules"
			       					/>
			       					<v-text-field 
			       						outlined
			       						type="text"
			       						label="C - variant"
			       						color="teal accent-3"
			       						prepend-icon="mdi-alphabetical-variant"
			       						v-model="form.c"
			       						:rules="rules"
			       					/>
			       					<v-text-field 
			       						outlined
			       						type="text"
			       						label="D - variant"
			       						color="teal accent-3"
			       						prepend-icon="mdi-alphabetical-variant"
			       						v-model="form.d"
			       						:rules="rules"
			       					/>
			       					<v-switch
			       						color="teal accent-3"
			       						label="trash" 
			       						v-model="form.trash"
			       					/>
			       					<div>		
				       					<v-select
								          :items="languages"
								          item-text="title"
								          label="Language"
								          outlined
								          v-model="form.language"
								          :rules="rules"
							        	></v-select>
			       					</div>
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
      				v-model="editor"
      				max-width="600"

    			 >
      				<v-card color="#1b2a47" class="pa-10 text-center" dark>
	       				<v-form @submit.prevent="editQuest(questid.id)" v-model="valid">	       						
		       				<v-card-text>	
								<h3>Editing Question</h3>
		       				</v-card-text>
		       				<v-card-text>
			       					<v-text-field
			       						outlined
			       						type="text"
			       						label="Question"
			       						color="teal accent-3"
			       						prepend-icon="mdi-format-text"
			       						v-model="edited.qestion"
			       						:rules="rules"
			       					/>
			       					<v-text-field 
			       						outlined
			       						type="text"
			       						label="Answer"
			       						color="teal accent-3"
			       						prepend-icon="mdi-ab-testing"
			       						v-model="edited.answer"
			       						:rules="rules"

			       					/>
			       					<v-text-field 
			       						outlined
			       						type="text"
			       						label="A - variant"
			       						color="teal accent-3"
			       						prepend-icon="mdi-alphabetical-variant"
			       						v-model="edited.a"
			       						:rules="rules"

			       					/>
			       					<v-text-field 
			       						outlined
			       						type="text"
			       						label="B - variant"
			       						color="teal accent-3"
			       						prepend-icon="mdi-alphabetical-variant"
			       						v-model="edited.b"
			       						:rules="rules"

			       					/>
			       					<v-text-field 
			       						outlined
			       						type="text"
			       						label="C - variant"
			       						color="teal accent-3"
			       						prepend-icon="mdi-alphabetical-variant"
			       						v-model="edited.c"
			       						:rules="rules"

			       					/>
			       					<v-text-field 
			       						outlined
			       						type="text"
			       						label="D - variant"
			       						color="teal accent-3"
			       						prepend-icon="mdi-alphabetical-variant"
			       						v-model="edited.d"
			       						:rules="rules"

			       					/>
			       					<v-switch
			       						color="teal accent-3"
			       						label="trash" 
			       						v-model="edited.trash"
			       					/>
			       					<div>		
				       					<v-select
								          :items="languages"
								          item-text="title"
								          label="Language"
								          outlined
								          v-model="edited.language"
			       						  :rules="rules"
							        	></v-select>
			       					</div>
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
		head: {
      		title: 'Admin Panel - Questions'
    	},
		data(){
			return{
				rules: [
        			value => !!value || 'This field is required.',
        			value => (value && value.length >= 5) || 'Min 5 characters',
      			],
      			valid: true,
				allQuestions: [],
				languages: [],
				questid: null,
				dialog: false,
				editor: false,
				form: {
					qestion: '',
					answer: '',
					file: null,
					a: '',
					b: '',
					c: '',
					d: '',
					trash: false,
					language: null
				},
				edited: {
					qestion: '',
					answer: '',
					file: null,
					a: '',
					b: '',
					c: '',
					d: '',
					trash: false,
					language: null, 
				}
			}
		},
		methods:{
			get_questions(){
				 this.$axios.get('http://127.0.0.1:8000/api/question-view/')
					.then(res => {
						let data = res.data
						this.allQuestions = data
					})
			},
			get_language(){
				this.$axios.get('http://127.0.0.1:8000/api/language-view/')
				.then(res => {
					let lang = res.data
					this.languages = lang
				})
			},
			async postQuest(){
				this.$axios.$post('http://127.0.0.1:8000/api/question-create/', this.form)
					.then(result => {
						this.allQuestions.push(result)
						this.dialog = false
						this.form.qestion = ''
						this.form.answer = ''
						this.form.file = null
						this.form.a = ''
						this.form.b = ''
						this.form.c = ''
						this.form.d = ''
						this.form.language = null
					})
					.catch(err => {
						console.log(err)
					})
			},
			async editQuest(id){
				await this.$axios.$post(`http://127.0.0.1:8000/api/question-update/${id}/`, this.edited)
					.then(res => {
						this.allQuestions = res.data
						this.get_questions()
						this.editor = false
					})
			}

		},
		created(){
			this.get_questions()
			this.get_language()
		}
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
	.font{
		font-family: Ubunutu;
	}
	h3{
		font-family: Ubuntu;
	}
</style>