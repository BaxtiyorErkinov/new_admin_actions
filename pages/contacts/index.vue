<template>
	<div>
		<v-container fluid class="my-5">
			<v-row justify="center">
				<v-col cols="12" sm="6" md="4" lg="3" v-for="person in allContacts" :key="person.name">
					<v-card flat class="text-center cursor" color="#1b2a47" dark>
						<v-responsive class="pt-4">
							<v-avatar size="100" class="grey lighten-2">
								<img src="@/assets/images/contact.png" alt="as" />
							</v-avatar>
						</v-responsive>
						<v-card-subtitle>
							<div class="text-h6">
								<v-icon small>mdi-account</v-icon>
								{{ person.text }}
							</div>
						</v-card-subtitle>
						<v-card-text class="hovered">
							<div class="grey--text">
								<v-icon small>mdi-phone</v-icon>
							{{ person.number }}
							</div>
						</v-card-text>
					</v-card>
				</v-col>
			</v-row>
		</v-container>
	</div>
</template>

<script>
	export default{
		head: {
     		title: 'Admin Panel - Contacts'
    	},
		data(){
			return{
				allContacts: [],
			}
		},
		methods: {
		  get_contacts () {
		    this.$axios.get('http://127.0.0.1:8000/api/contact-view/')
		 		.then(res => {
		 			let data = res.data
		 			this.allContacts = data
		 		})   
		 	}
		},
		created(){
			this.get_contacts()
		}
	}
</script>

<style scoped>
	.cursor{
		cursor: pointer;
	}
	.hovered:hover{
		color: red;
	}
	h2{
		font-family: Ubuntu;
	}
</style>