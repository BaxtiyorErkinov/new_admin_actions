<template>
	<nav class="color">
		<v-app-bar flat app color="#1b2a47">
			<v-app-bar-nav-icon @click="drawer = !drawer" color="white"></v-app-bar-nav-icon>
				<v-toolbar-title class="center display-1">
					<nuxtLink to="/Login" class="titl">Admin<span>Panel</span></nuxtLink>
				</v-toolbar-title>
		</v-app-bar>
		<v-navigation-drawer app v-model="drawer" color="#1b2a47" width="200">
				<v-row wrap class="mt-8">
					<v-list-item link to="/student">
							<v-col cols="12" class="d-flex justify-center white--text">
								<v-icon left color="white">mdi-account</v-icon>
								<h4 class="ml-4">Students</h4>
							</v-col>
<!-- 							<v-col cols="12" class="d-flex justify-center white--text">
								<v-menu offset-y color="#152036" class="zz" transition="slide-x-transition" dark>
									<template v-slot:activator="{ on, attrs }">
										<v-icon left color="white">mdi-account</v-icon>
										<h4 v-on="on">Students</h4>
									</template>	
								        <v-list-item class="backg" link to="/student">
									        <v-col cols="12">
									          <v-list-item-title color="#152036">Student</v-list-item-title>
									        </v-col>
									    </v-list-item>
									    	<v-divider />
									    <v-list-item class="backg" link to="/student">							        
										    <v-col cols="12">
										        <v-list-item-title>Overall</v-list-item-title>
										    </v-col>
									    </v-list-item>	
									    	<v-divider />
									    <v-list-item class="backg" link>								    	
									        <v-col cols="12">
									          <v-list-item-title>Create</v-list-item-title>
									        </v-col>
									    </v-list-item>
									    	<v-divider />
									    <v-list-item class="backg" link>								    	
									        <v-col cols="12">
									          <v-list-item-title>Update</v-list-item-title>
									        </v-col>
									    </v-list-item>							
								</v-menu>
							</v-col> -->
					</v-list-item>
					<v-list-item link to="/center">				
							<v-col cols="12" class="d-flex justify-center white--text">
								<v-icon left color="white">mdi-school</v-icon>
								<h4 class="ml-4">Center</h4>
							</v-col>
					</v-list-item>
					<v-list-item link to="/languages">				
							<v-col cols="12" class="d-flex justify-center white--text">
								<v-icon left color="white">mdi-globe-model</v-icon>
								<h4 class="ml-2">Language</h4>
							</v-col>
					</v-list-item>
					<v-list-item link to="/questions">				
							<v-col cols="12" class="d-flex justify-center white--text">
								<v-icon left color="white">mdi-comment-question-outline</v-icon>
								<h4>Questions</h4>
							</v-col>
					</v-list-item>
					<v-list-item link to="/contacts">				
							<v-col cols="12" class="d-flex justify-center white--text">
								<v-icon left color="white">mdi-contacts</v-icon>
								<h4>Contacts</h4>
							</v-col>
					</v-list-item>
					<v-list-item link to="/certificates">				
							<v-col cols="12" class="d-flex justify-center white--text">
								<v-icon left color="white">mdi-certificate-outline</v-icon>
								<h4>Certificates</h4>
							</v-col>
					</v-list-item>
					<v-list-item link to="/books">				
							<v-col cols="12" class="d-flex justify-center white--text">
								<v-icon left color="white">mdi-book-open-page-variant-outline</v-icon>
								<h4>Books</h4>
							</v-col>
					</v-list-item>
					<v-list-item link to="/news">				
							<v-col cols="12" class="d-flex justify-center white--text">
								<v-icon left color="white">mdi-bell-badge-outline</v-icon>
								<h4>News</h4>
							</v-col>
					</v-list-item>
				</v-row>
		</v-navigation-drawer>
		</v-navigation-drawer>
	</nav>
</template>

<script>
	export default{
		data(){
			return{
				drawer: false,
				links: [
					{ icon: 'mdi-account' },
					{ icon: 'mdi-school' },
					{ icon: 'mdi-globe-model' },
					{ icon: 'mdi-comment-question-outline' },
					{ icon: 'mdi-contacts' },
				]
			}
		}
	}
</script>
<style scoped>
	.titl{
		text-decoration: none;
		color: rgb(104, 255, 74);
	}
	.color{
		background-color: #152036;
	}
	.backg{
		background-color: #152036;
	}
	.center{
		color:  rgb(104, 255, 74);
		margin: 0 auto;
	}
	span{
		color: #fff;
	}
</style>