<script>
import axios from 'axios';
import swal from 'sweetalert';
import Nav from "../components/nav.vue";
import Jumbotron from "../components/jumbotron.vue";
import Modal from "../components/modal.vue";
import StarRating from 'vue-star-rating'
export default{
    components: {
		Nav,
        Jumbotron,
        Modal,
        StarRating
	},
    computed: {
        displayedValues() {
            return this.recommendedRestaurants.slice(0, this.displayedCount);
        },
  },
    mounted() {
        this.parseJwt(this.token);
        this.getRevisitedRestaurant(this.userID);
        this.getRecommendedRestaurant(this.userID);
    },
    updated(){
       // this.getRevisitedRestaurant(this.userID);
    },

    data() {
        return {
           token: localStorage.getItem('token'),
           modal:false,
           rating:0,
           userID:'',
           revisitedRestaurants:[],
           recommendedRestaurants:[],
           recomRes:[],
           errorMsg:'',
           displayedCount: 5,
           formData:{
                userID:'',
                placeID:'',
                food_rating:0,
                service_rating:0
           }
        }
    },
    methods: {
        parseJwt(token) {
            var base64Url = token.split('.')[1];
            var base64 = base64Url.replace(/-/g, '+').replace(/_/g, '/');
            var jsonPayload = decodeURIComponent(window.atob(base64).split('').map(function(c) {
                return '%' + ('00' + c.charCodeAt(0).toString(16)).slice(-2);
            }).join(''));
            this.userID=JSON.parse(jsonPayload).userID;
        },
        scroll(e) {
            if (e.deltaY > 0) this.$refs.container.scrollLeft += 80;
            else this.$refs.container.scrollLeft -= 80;
        },
        scrollLeft() {
            this.$refs.container.scrollLeft -= 100;
        },
        scrollRight() {
            this.$refs.container.scrollLeft += 100;
        },
        openModal(id){
            this.formData.placeID=id;
            this.modal=true;
           
        },
        closeModal(){
            this.modal=false;
        },
        async getRevisitedRestaurant(id) {
            await axios.get('http://localhost:8000/api/dinein/most-visited/' + id, {
                headers: {
                "Authorization": `Bearer ${localStorage.getItem('token') }`
                }
            })
            .then((response) => {
                this.revisitedRestaurants = response.data['data'];
            })
            .catch((error) => {
                console.log(error)
                this.errorMsg = 'Error retrieving data'
            })
      },
      async getRecommendedRestaurant(id) {
            await axios.get('http://localhost:8000/api/dinein/recommend/' + id, {
                headers: {
                "Authorization": `Bearer ${localStorage.getItem('token') }`
                }
            })
            .then((response) => {
                this.recommendedRestaurants = response.data['data'];
            })
            .catch((error) => {
                console.log(error)
                this.errorMsg = 'Error retrieving data'
            })
      },
      async submitReview() {
			if (this.formData.food_rating>=1 && this.formData.service_rating>=1) {
                this.formData.userID=this.userID;
				await axios.post(
					'http://localhost:8000/api/dinein/rating/add', this.formData, {
                    headers: {
                    "Authorization": `Bearer ${localStorage.getItem('token') }`
                    }
                    })
                    .then(() => {
                        swal({
                            title: "Success",
                            text: "Review given successfully!",
                            icon: "success",
                            timer: 1000,
                            buttons: false
                        });
                        this.getRevisitedRestaurant(this.userID);
                        this.getRecommendedRestaurant(this.userID);
                        this.formData.food_rating=0;
                        this.formData.service_rating=0;
                        this.closeModal();
                    })
					.catch((error) => {
						swal({
							title: "Invalid Information",
							text: "Invalid Rating and Information",
							icon: "error",
							buttons: true,
						})
					})
			}
		},
      loadMore() {
        this.displayedCount += 5;
    },
    }
}

</script>
<template>
    <div class="bg-rose-50">
        <div class="py-3">
            <Nav :userID="userID"/>
        </div>
        <div class="hidden lg:block lg:px-8 lg:py-8 ">
            <Jumbotron/>
        </div>
        <div>
            <!-- component -->
            <section class="antialiased bg-gray-100 text-gray-600  px-4 py-4 font-mono h-screen" x-data="app">

                <!-- Revisited List -->
                <div>
                    <div class="flex justify-between">
                        <p class="text-left text-xl text-gray-800 py-2 px-2">Take a glance of the <span class="font-semibold text-brownish-red">restaurants already visited</span> by you !!!</p>
                        <p class="text-left text-sm text-gray-800 py-2 px-2 flex justify-end">Total results showing - <span class="font-semibold text-brownish-red ml-0.5">{{ this.revisitedRestaurants.length}}</span></p>
                    </div>
                    <div v-if="this.revisitedRestaurants.length>=1" class="flex border border-brownish-red-light border-opacity-40  bg-brownish-light bg-opacity-20 shadow-md ">
                        <div  class="h-full w-1/12 flex justify-center items-center py-14">
                            <button v-show="this.revisitedRestaurants.length>=4" class="cursor-pointer" @click="scrollLeft($event)">
                                <img class="h-8 w-8 fill-current bg-brownish-red-light rounded-full" src="https://img.icons8.com/external-inkubators-detailed-outline-inkubators/50/000000/external-left-chevron-arrows-inkubators-detailed-outline-inkubators.png"/>            
                            </button>
                        </div>

                        <div class="py-3 font-mono flex w-10/12 overflow-x-scroll no-scrollbar hide-scroll-bar scroll-smooth" ref="container">
                            <div v-for ="(item,index) in this.revisitedRestaurants" :key="index" class="max-w-md py-3 px-4 mx-2 bg-white shadow-lg rounded-lg my-5 w-60 shrink-0" @wheel="scroll($event)">
                                <div class="flex justify-center md:justify-start -mt-7">
                                    <div class="w-10 h-10 object-cover rounded-full border-2 border-brownish-red-dark bg-brownish-red text-white text-center"><span class="">{{index+1}}</span></div>
                                </div>
                                <div class="py-2 flex justify-between">
                                    <div v-if="item.cusine" class="text-gray-800 text-base font-semibold">{{ item.cusine.rcuisine}}</div>
                                    <div v-else class="text-gray-800 text-base font-semibold">-</div>
                                    <p class="text-gray-600 text-xs py-0.5">Overall Rating</p>
                                </div>
                                <div class="flex justify-between">
                                    <p class="text-sm text-gray-500">PlaceID- <span class="text-sm font-semibold text-brownish-red-darker mt-1">{{ item.placeID }}</span></p>
                                    <div class="flex">
                                        <div class="py-0.5">
                                            <img class="h-4 w-4 " src="https://img.icons8.com/color/48/null/filled-star--v1.png"/>
                                        </div>
                                    
                                        <p class="text-sm font-bold text-green-400 px-0.5 font ">{{item.overall_rating }}</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div  class="h-full w-1/12 flex justify-center items-center py-14">
                            <button v-show="this.revisitedRestaurants.length>=4" class="cursor-pointer"  @click="scrollRight($event)">
                                <img class="h-8 w-8 fill-current bg-brownish-red-light rounded-full" src="https://img.icons8.com/ios/50/null/circled-chevron-right--v1.png"/>
                            </button>
                        </div>
                    </div>
                    <div v-else class="text-gray-600 text-lg px-2">No results to be shown...</div>
                </div>

                <!-- Recommendation List -->
                <div class="py-10">
                    <article class="mb-4">

                        <p class="text-left text-xl text-brownish-red font-semibold py-2 px-2">Are you craving an unforgettable dining experience?</p>
                        <div class="flex justify-between">
                            <div>
                                <p class="text-base text-gray-500 px-2 font-normal pb-3">These options are definitely worth a try!</p>
                            </div>
                            <!-- <div class="px-2"> -->
                                <button class=" border-2 rounded px-2 py-1 border-brownish-red-dark  bg-white" @click="loadMore">
                                    <span class="text-sm font-semibold text-gray-600 hover:text-brownish-red-dark">Load More</span> 
                                </button>
                            <!-- </div> -->
                        </div>
                    </article>

                    <div v-if="this.recommendedRestaurants.length>=1" class="flex border border-brownish-red-light border-opacity-40 bg-brownish-light bg-opacity-20 shadow-md ">
                        <div  class="h-full w-1/12 flex justify-center items-center py-14">   
                        </div>
                        <div class="py-3 font-mono grid grid-cols-5  w-10/12">
                            <div v-for ="(item,index) in this.displayedValues" :key="index" class="max-w-md py-3 px-4 mx-2 bg-white shadow-lg rounded-lg my-5 w-56 shrink-0">
                                <div class="flex justify-center md:justify-start -mt-7">
                                    <div class="w-10 h-10 object-cover rounded-full border-2 border-brownish-red-dark bg-brownish-red text-white text-center"><span class="">{{index+1}}</span></div>
                                </div>
                                <div class="py-2 flex justify-between">
                                    <h2 class="text-gray-800 text-sm font-semibold pr-8">{{item.Rcuisine}}</h2>
                                    <span class="text-sm font-semibold text-brownish-red-darker mt-1">{{ item.placeID}}</span>
                                </div>
                                <div class="py-2">
                                    <h2 class="text-gray-600 text-xs font-semibold pr-8"><span class="text-gray-500 font-medium"> Place score -  </span>{{item.place_score.toFixed(3)}}</h2>
                                    <div class="text-gray-600 text-xs font-semibold pr-8 flex">
                                        <div class="text-gray-500 font-medium"> Predicted score -</div>
                                        <img class="h-4 w-4 " src="https://img.icons8.com/color/48/null/filled-star--v1.png"/>
                                        <div class="text-red-500">{{item.predicted_rating.toFixed(3)}}</div>
                                    </div>
                                    
                                </div>
                                <div class="flex justify-end py-2">
                                    <button class="border hover:shadow-sm hover:border-2 shadow border-green-400 px-3 py-0.5 rounded-full text-xs font-semibold text-green-400 "  @click="openModal(item.placeID)">Rate</button>
                                </div>
                            </div>
                        </div>
                        <div  class="h-full w-1/12 flex justify-center items-center py-14">
                        </div>
                    </div>
                    <div v-else class="text-gray-600 text-lg px-2 ">No results to be shown...</div>
                </div>
                <!-- Modal -->
                <Modal :width="'w-1/3'" v-show="this.modal" @close="closeModal">
                    <template v-slot:header>
                        <h3 class="">Review Restaurant</h3>
                    </template>
                    <template v-slot:body>
                        <div class="py-2 pb-4">
                            <!-- flex justify-center -->

                            <tr class="flex items-center justify-center">
                                <td class="text-xl font-mono px-2 mt-1 text-left">Food Rating - </td>
                                <td>
                                    <star-rating v-model:rating="this.formData.food_rating" v-bind:increment="1"
                                        v-bind:max-rating="5"
                                        inactive-color="#A9A9A9"
                                        active-color="#f9ac1b"
                                        v-bind:star-size="20"
                                        v-bind:show-rating=false>
                                    </star-rating>
                                </td>
                            </tr>

                            <tr class="flex items-center justify-center">
                                <td class="text-xl font-mono text-left px-2 mt-1"> Service Rating - </td>
                                <td>
                                    <star-rating  v-model:rating="this.formData.service_rating" v-bind:increment="1"
                                        v-bind:max-rating="5"
                                        inactive-color="#A9A9A9"
                                        active-color="#f9ac1b"
                                        v-bind:star-size="20"
                                        v-bind:show-rating=false
                                        clearable=true>
                                    </star-rating>

                                </td>
                            </tr>

                        </div>
                    </template>
                    <template  v-slot:footer>
                        <div class="flex justify-center py-6">
                            <button @click="submitReview()" class=" border-2 rounded px-2 py-1 border-brownish-red-dark bg-white">
                                <span class="text-base font-semibold text-gray-600 hover:text-brownish-red-dark">Submit</span> 
                            </button>
                        </div>
                    </template>
                </Modal>
            </section>
        </div>
    </div>
</template>

<style scoped>
/* Hide scrollbar for Chrome, Safari and Opera */
.no-scrollbar::-webkit-scrollbar {
    display: none;
}
/* Hide scrollbar for IE, Edge and Firefox */
.no-scrollbar {
    -ms-overflow-style: none;
    /* IE and Edge */
    scrollbar-width: none;
    /* Firefox */
}

</style>