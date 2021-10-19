<template>
	<nav class="color">
		<v-app-bar flat app color="#1b2a47">
			<v-app-bar-nav-icon @click="drawer = !drawer" color="white"></v-app-bar-nav-icon>
				<v-toolbar-title class="center display-1">
					<nuxtLink to="/admin/Login" class="titl">Admin<span>Panel</span></nuxtLink>
				</v-toolbar-title>
		</v-app-bar>
		<v-navigation-drawer app v-model="drawer" color="#1b2a47" width="200">
				<v-row wrap class="mt-8" v-for="item in links" :key="item.id">
					<v-list-item link :to="item.rout">
							<v-col cols="12" class="d-flex justify-center white--text">
								<v-icon left color="white">{{item.icon}}</v-icon>
								<h4 class="ml-4">{{item.title}}</h4>
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
					{ id: 1, title: 'Student', icon: 'mdi-account', rout: '/admin/student' },
					{ id: 2, title: 'Center', icon: 'mdi-school',rout: '/admin/center' },
					{ id: 3, title: 'Language', icon: 'mdi-globe-model', rout: '/admin/languages' },
					{ id: 4, title: 'Question', icon: 'mdi-comment-question-outline', rout: '/admin/questions' },
					{ id: 5, title: 'Contacts', icon: 'mdi-contacts', rout: '/admin/contacts' },
					{ id: 6, title: 'Certificates', icon: 'mdi-certificate-outline', rout: '/admin/certificates' },
					{ id: 7, title: 'Books', icon: 'mdi-book-open-page-variant-outline', rout: '/admin/books' },
					{ id: 8, title: 'News', icon: 'mdi-bell-badge-outline',rout: '/admin/news' },
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