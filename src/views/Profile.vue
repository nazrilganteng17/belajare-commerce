<template>
<div class="max-w-4xl flex items-center h-auto lg:h-screen flex-wrap mx-auto my-32 lg:my-0 ">
    
	<!--Main Col-->
	<div id="profile" class="w-full lg:w-3/5 rounded-lg lg:rounded-l-lg lg:rounded-r-none  bg-white opacity-75 mx-6 lg:mx-0">
	

		<div class="p-4 md:p-12 text-center lg:text-left">
			<!-- Image for mobile view-->
			<div class="block lg:hidden rounded-full mx-auto -mt-16 h-48 w-48 bg-cover bg-center" style="background-image: url('https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSrdO9t4Q0GbUvaN_4YGJvxspWMoWAiHv3h1A&usqp=CAU')"></div>
			
			<h1 class="text-3xl font-bold pt-8 lg:pt-0">{{ getUser.name }}</h1>
			<div class="mx-auto lg:mx-0 w-4/5 pt-3 border-b-2 border-teal-500 opacity-25"></div>
			<p class="pt-4 text-base font-bold flex items-center justify-center lg:justify-start"><img class="mr-3" src="https://cdn-icons-png.flaticon.com/128/888/888853.png" width="30" height="30">Website Builder</p>
			<p class="pt-2 text-black-600 text-xs lg:text-sm flex items-center justify-center lg:justify-start font-semibold" v-for="get in address.data"><img class="mr-3" src="https://img.icons8.com/officel/30/000000/worldwide-location.png"> <a href="https://www.google.co.id/maps/place/Bandung,+Kota+Bandung,+Jawa+Barat/@-6.903272,107.5607548,12z/data=!3m1!4b1!4m6!3m5!1s0x2e68e6398252477f:0x146a1f93d3e815b2!8m2!3d-6.9174639!4d107.6191228!16zL20vMDF6bGw4?hl=id&entry=ttu">
        {{ get.address }},
        {{ get.city }}, {{ get.state }}, {{ get.postal_code }}
      </a></p>
		
    
			<div class="pt-12 pb-8">
				<button class="bg-teal-700 hover:bg-blue-800 hover:text-white text-black font-bold py-2 px-4 rounded-full">
				  Get In Touch
				</button> 
			</div>

			<div class="mt-6 pb-16 lg:pb-0 w-4/5 lg:w-full mx-auto flex flex-wrap items-center justify-between">
        
				<a class="link opacity-100 hover:opacity-50" href="https://www.facebook.com/nazrililham.aldino.39/" target="_blank"><img src="https://img.icons8.com/color/30/000000/facebook-circled.png"></a>
				<a class="link opacity-100 hover:opacity-50" href="https://twitter.com/home?lang=id" target="_blank"><img src="https://img.icons8.com/color/30/000000/twitter-circled.png"></a>
				<a class="link opacity-100 hover:opacity-50" href="https://www.instagram.com/nazrlllham" target="_blank"><img src="https://img.icons8.com/cute-clipart/30/000000/instagram-new.png"></a>
 				<a class="link opacity-100 hover:opacity-50" href="https://wa.link/pix9qo" target="_blank"><img src="https://cdn-icons-png.flaticon.com/128/2111/2111728.png" width="30" height="30"></a>
 				<a class="link opacity-100 hover:opacity-50" href="https://github.com/nazrilganteng17" target="_blank"><img src="https://img.icons8.com/ios-filled/30/000000/github.png"></a>
			</div>

		</div>


	</div>
	
	<!--Img Col-->
	<div class="w-full lg:w-2/5">
		<!-- Big profile image for side bar (desktop) -->
		<img src="https://d7hftxdivxxvm.cloudfront.net/?height=800&quality=80&resize_to=fit&src=https%3A%2F%2Fd32dm0rphc51dk.cloudfront.net%2FK1m-ug6N-GPJg8Y66UWJSA%2Fnormalized.jpg&width=498" class="rounded-none lg:rounded-lg hidden lg:block" height="250" width="250">
		<!-- Image from: http://unsplash.com/photos/MP0IUfwrn0A -->
		
	</div>


</div>
</template>
<script>
import { mapGetters, mapActions } from 'vuex';

export default {
  computed: {
      ... mapGetters('user', ['getUser']),
      ... mapGetters('auth', ['getUserAddress']),

      address() {
        return this.getUserAddress
      }
  },
  methods: {
      ...mapActions('user', ["fetchUser"]),
      ...mapActions('auth', ["gettersUserAddress"]),
  },
  async mounted() {
    this.gettersUserAddress();
    const user = await this.fetchUser();

    if (user) {
      this.$store.commit('profile/SET_USER', user);
    }
  },
  created() {
      this.fetchUser();
  },
}
</script>