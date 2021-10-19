<template>
	<div>
		<v-container fluid class="my-5">
			<v-row justify="center">
				<v-col cols="12" sm="6" md="5" lg="4" v-for="centers in educenters" :key="centers.id"> 
					<v-card flat class="text-center cursor d-flex pa-5" color="#1b2a47" dark> 
						<v-row>
							<v-col cols="6">
								<div>						
									<v-responsive>
										<v-avatar size="100" class="grey lighten-2">
											<img :src="`https://actions.uz${centers.logo}`" alt="as" />
										</v-avatar>
									</v-responsive>
								</div>
							</v-col>
							<v-col cols="6">
								<div class="mt-3">
									<div>
										<h3>									
											<v-icon class="mr-2">mdi-school</v-icon>
											{{ centers.title }}
										</h3>
										<h3 class="mt-3">
											<v-icon class="mr-4">mdi-city</v-icon>
											{{centers.region}}
										</h3>
									</div>
							</div>
							</v-col>
						</v-row>
<!-- 						<div>						
							<v-responsive>
								<v-avatar size="100" class="grey lighten-2">
									<img :src="`http://127.0.0.1:8000${centers.logo}`" alt="as" />
								</v-avatar>
							</v-responsive>
						</div>
						<div>
							
							<v-divider  vertical class="ml-15 mr-3"/>
						</div>
						<div>
								<div>
							<v-card-subtitle>
								<h3>									
									<v-icon class="mr-2">mdi-school</v-icon>
									{{ centers.title }}
								</h3>
								<h3 class="mt-2">
									<v-icon class="mr-4">mdi-city</v-icon>
									{{centers.region}}
								</h3>
							</v-card-subtitle>
								</div>
						</div> -->
					</v-card>
				</v-col>
			</v-row>
		</v-container>
	</div>
</template>

<script>
	export default{
		head: {
      		title: 'Admin Panel - Centers'
    	},
		data(){
			return{
				educenters: [],
			}
		},
		methods:{
			get_centers(){
				this.$axios.get('https://actions.uz/api/educenter-view/')
					.then(res => {
						let data = res.data
						this.educenters = data
					})
			},
		},
		created(){
			this.get_centers()
		}
	}
</script>

<style scoped>
	.cont{
		margin: 100px;
	}
</style>