<script>
import Nav from "../components/nav.vue";
import axios from "axios";
export default{
    components: {
		Nav
	},
    computed: {
        displayedValues() {
            if(this.recommendedRestaurants.length<this.displayedCount){
                this.displayedCount=this.recommendedRestaurants.length
            }
            return this.recommendedRestaurants.slice(0, this.displayedCount);
        },
  },
    created(){
       this.parseJwt(this.token);
       this.getRecommendedRestaurant(this.userID);
    },
	data(){
		return{
            length:0,
            token: localStorage.getItem('token'),
            recommendedRestaurants:[],
            open:false,
            show:false,
            displayedCount: 10,
            input: {
                userID:'',
                placeID:'',
                food_rating: 0,
                service_rating:0

            },
            inputs:[
                {
                    placeID:''
                }
            ],
            percentage:0,
            userID:'',
            testCases:[],
            efficiency_score:0
		}
	},
	methods:{
        parseJwt(token) {
            var base64Url = token.split('.')[1];
            var base64 = base64Url.replace(/-/g, '+').replace(/_/g, '/');
            var jsonPayload = decodeURIComponent(window.atob(base64).split('').map(function(c) {
                return '%' + ('00' + c.charCodeAt(0).toString(16)).slice(-2);
            }).join(''));
            this.userID=JSON.parse(jsonPayload).userID;
        },
        add () {
            this.inputs.push({
                placeID:''
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
			if (this.input.food_rating>=1 && this.input.service_rating>=1) {
                this.input.userID=this.userID;
                console.log(this.input)
				await axios.post(
					'http://localhost:8000/api/dinein/rating/add', this.input, {
                    headers: {
                    "Authorization": `Bearer ${localStorage.getItem('token') }`
                    }
                    })
                    .then(() => {
                        swal({
                            title: "Success",
                            text: "Revisied restaurant added successfully!",
                            icon: "success",
                            timer: 1000,
                            buttons: false
                        });
                        //this.getRevisitedRestaurant(this.userID);
                        this.getRecommendedRestaurant(this.userID);
                        //length= this.reco
                        this.input.placeID='';
                        this.input.food_rating=0;
                        this.input.service_rating=0;
                        this.open=true;
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
        processEfficiencyScore(){
            this.percentage=0;
            this.efficiency_score=0;
            const testCases = [];
            const recommend = new Set();
            this.inputs.forEach(element => {
                // this.testCases.push(element.placeID);
                testCases.push(element["placeID"]);
            });

            this.recommendedRestaurants.forEach(el=>{
                recommend.add(""+el.placeID);
            });
            
            // test_cases = ['132723','132856','135041','135060','132754','135042','135052','135032','132572','135046','135071','135053','135099','135098']
            
            let num_tests_passed =0;
            const recommendArr = Array.from(recommend);
            
            recommendArr.forEach(pid => {     
                if(testCases.includes(pid)){
                    num_tests_passed = num_tests_passed+1
                    console.log(num_tests_passed)
                }
           });
            this.efficiency_score = num_tests_passed/ testCases.length;
            this.percentage=this.efficiency_score*100;
            this.show=true;
            this.inputs=[
                {
                    placeID:''
                }
            ]
        },

        remove (index) {
        this.inputs.splice(index, 1)
        }
	}
}
</script>

<template>
    <div class="">
        <div class="">
            <Nav :userID="userID"/>
        </div>
        <div class="max-w-2xl mx-auto p-16 border border-brownish-red border-opacity-30 font-mono bg-brownish-red-dark bg-opacity-20 my-16">
            <div id=app>
                <label class="block mb-2 text-base font-medium text-gray-800">Enter the restaurant name you already revisited: </label>
                <div class="flex">
                    <div class="w-1/4 my-1">
                        <label class="text-sm text-gray-700 font-semibold  block mx-1">Place ID</label>
                    </div>
                    <div class="w-1/4 my-1">
                        <label class="text-sm text-gray-700 font-semibold  block mx-1">Food Rating</label>
                    </div>
                    <div class="w-1/4 my-1">
                        <label class="text-sm text-gray-700 font-semibold  mx-1 block">Service Rating</label>
                    </div>
                    <div class="w-1/4 my-2 mx-1 hidden">
                        <button ><img class="w-8 h-8" src="https://img.icons8.com/color/48/null/cancel--v1.png"/></button>
                        <button ><img class="w-8 h-8" src="https://img.icons8.com/color/48/null/add--v1.png"/></button>
                    </div>
                    
                </div>
                <div class="flex" >
                    <div class="w-1/4 my-1">
                        <input v-model="input.placeID" type="text" id="placeID" class=" bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg block w-full p-2.5 focus:outline-none" placeholder="Place ID" required>      
                    </div>
                    <div class="w-1/4 my-1">
                        <input v-model="input.food_rating"  id="foood_rating" class=" bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg block w-full p-2.5 focus:outline-none" placeholder="Food Rating" required>      
                    </div>
                    <div class="w-1/4 my-1">
                        <input v-model="input.service_rating" id="service_rating" class=" bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg block w-full p-2.5 focus:outline-none" placeholder="Service Rating" required>      
                    </div>
                    <div class="w-1/4 my-2 mx-1">
                        <button class=" border-2 rounded px-2 py-1 border-brownish-red-dark  bg-white" @click="submitReview">
                            <span class="text-sm font-semibold text-gray-600 hover:text-brownish-red-dark">Add</span> 
                        </button>
                    </div>
                    
                </div>
                
                <div v-if="this.recommendedRestaurants.length>=1" class="py-2 border border-brownish-red-dark border-opacity-25 rounded mt-4 px-2">
                    <label class="block mb-2 text-base font-medium underline text-gray-700">The recommended restaurant for user- <span class="text-brownish-red-dark">{{this.userID }}</span>: </label>
                    <div class="flex flex-col">
                        <div class="overflow-x-auto sm:-mx-6 lg:-mx-8">
                            <div class="inline-block min-w-full py-2 sm:px-6 lg:px-8">
                                <div class="overflow-hidden">
                                    <table class="min-w-full text-left text-sm font-light">
                                    <thead class="border-b font-medium dark:border-neutral-500">
                                        <tr>
                                        <th scope="col" class="px-6 py-4">#</th>
                                        <th scope="col" class="px-6 py-4">Place ID</th>
                                        <th scope="col" class="px-6 py-4">Cuisine Name</th>
                                        <th scope="col" class="px-6 py-4">Place Score</th>
                                        <th scope="col" class="px-6 py-4">Predicted Rating</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="(item,index) in this.displayedValues" :key="index" class="border-b dark:border-neutral-500">
                                            <td class="whitespace-nowrap px-6 py-4 font-medium">{{index+1}}.</td>
                                            <td class="whitespace-nowrap px-6 py-4">{{ item.placeID }}</td>
                                            <td class="whitespace-nowrap px-6 py-4">{{ item.Rcuisine }}</td>
                                            <td class="whitespace-nowrap px-6 py-4">{{ item.place_score.toFixed(2)}}</td>
                                            <td class="whitespace-nowrap px-6 py-4">{{ item.predicted_rating.toFixed(0)}}</td>
                                        </tr>
                                    </tbody>
                                    </table>
                                </div>
                           </div>
                      </div>
                    </div>
                </div>

                <label class="block mb-2 text-base font-medium text-gray-800 mt-2">Enter the test case restaurants to calculate efficiency: </label>
                
                <div class="flex" v-for="(input,k) in inputs" :key="k">
                    <div class="w-3/4 my-1 mx-2">
                        <input v-model="input.placeID" type="text" id="placeID" class=" bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg block w-full p-2.5 focus:outline-none" placeholder="Place ID" required>      
                    </div>
                    <div class="w-1/4 my-2 mx-2 ">
                        <button  @click="remove(k)" v-show="k || ( !k && inputs.length > 1)"><img class="w-8 h-8" src="https://img.icons8.com/color/48/null/cancel--v1.png"/></button>
                        <button @click="add(k)" v-show="k == inputs.length-1"><img class="w-8 h-8" src="https://img.icons8.com/color/48/null/add--v1.png"/></button>
                    </div>
                </div>
                <button @click="processEfficiencyScore" class=" border-2 rounded px-2 py-1 border-brownish-red-dark  bg-white">
                    <span class="text-sm font-semibold text-gray-600 hover:text-brownish-red-dark">Submit</span> 
                </button>
                <div v-show="show" class="text-sm text-gray-700 font-semibold mt-2 block">
                    The efficiency score : <span class="text-brownish-red-dark">{{ this.efficiency_score.toFixed(3) }}</span>
                    Percentage : <span class="text-brownish-red-dark">{{ this.percentage}} % </span>
                </div>
            </div>
        </div>
    </div>
    <!-- <pre>{{ this.inputs }}</pre> -->
</template>

<style scoped>

</style>