<script>
import Nav from "../components/nav.vue";
import Jumbotron from "../components/jumbotron.vue";
import Modal from "../components/modal.vue";
export default{
    components: {
		Nav,
        Jumbotron,
        Modal
	},
    data() {
        return {
           x:10,
           modal:false
        }
    },
    methods: {
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
        openModal(){
            this.modal=true;
        },
        closeModal(){
            this.modal=false;
        }
    }

}

</script>
<template>
    <div class="bg-rose-50">
        <div class="py-3">
            <Nav />
        </div>
        <div class="hidden lg:block lg:px-8 lg:py-8 ">
            <Jumbotron/>
        </div>
        <div>
            <!-- component -->
            <section class="antialiased bg-gray-100 text-gray-600  px-4 py-4 font-mono h-screen" x-data="app">

                <!-- Revisited List -->
                <div>
                    <p class="text-left text-lg text-gray-800 py-2 px-2">Take a glance of the <span class="font-semibold text-brownish-red">restaurants already visited</span> by you !!!</p>
                    <div class="flex border border-brownish-red-light border-opacity-40  bg-brownish-light bg-opacity-20 shadow-md ">
                        <div  class="h-full w-1/12 flex justify-center items-center py-14">
                            <button v-show="x>=4" class="cursor-pointer" @click="scrollLeft($event)">
                                <img class="h-8 w-8 fill-current bg-brownish-red-light rounded-full" src="https://img.icons8.com/external-inkubators-detailed-outline-inkubators/50/000000/external-left-chevron-arrows-inkubators-detailed-outline-inkubators.png"/>            
                            </button>
                        </div>

                        <div class="py-3 font-mono flex w-10/12 overflow-x-scroll no-scrollbar hide-scroll-bar scroll-smooth" ref="container">
                            <div v-for ="i in x" :key="i" class="max-w-md py-3 px-4 mx-2 bg-white shadow-lg rounded-lg my-5 w-60 shrink-0" @wheel="scroll($event)">
                                <div class="flex justify-center md:justify-start -mt-7">
                                    <div class="w-10 h-10 object-cover rounded-full border-2 border-brownish-red-dark bg-brownish-red text-white text-center"><span class="">{{i }}</span></div>
                                </div>
                                <div class="py-2 flex justify-between">
                                    <h2 class="text-gray-800 text-base font-semibold">Spanish</h2>
                                    <p class="text-gray-600 text-xs py-0.5">Overall Rating</p>
                                </div>
                                <div class="flex justify-between">
                                    <p class="text-sm font-semibold text-brownish-red-darker mt-1">Pl00001</p>
                                    <div class="flex mt-1">
                                        <div class="py-0.5">
                                            <img class="h-4 w-4 " src="https://img.icons8.com/color/48/null/filled-star--v1.png"/>
                                        </div>
                                    
                                        <p class="text-sm font-bold text-green-400 px-0.5 font ">3.4</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div  class="h-full w-1/12 flex justify-center items-center py-14">
                            <button v-show="x>=4" class="cursor-pointer"  @click="scrollRight($event)">
                                <img class="h-8 w-8 fill-current bg-brownish-red-light rounded-full" src="https://img.icons8.com/ios/50/null/circled-chevron-right--v1.png"/>
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Recommendation List -->
                <div class="py-10">
                    <article class="mb-4">

                        <p class="text-left text-lg text-brownish-red font-semibold py-2 px-2">Are you craving an unforgettable dining experience?</p>
                        <div class="flex justify-between">
                            <div>
                                <p class="text-base text-gray-500 px-2 font-normal pb-3">These options are definitely worth a try!</p>
                            </div>
                            <!-- <div class="px-2"> -->
                                <button class=" border-2 rounded px-2 py-1 border-brownish-red-dark  bg-white">
                                    <span class="text-sm font-semibold text-gray-600 hover:text-brownish-red-dark">Load More</span> 
                                </button>
                            <!-- </div> -->
                        </div>
                    </article>

                    <div class="flex border border-brownish-red-light border-opacity-40 bg-brownish-light bg-opacity-20 shadow-md ">
                        <div  class="h-full w-1/12 flex justify-center items-center py-14">   
                        </div>

                        <div class="py-3 font-mono flex w-10/12">
                            <div v-for ="i in 5" :key="i" class="max-w-md py-3 px-4 mx-2 bg-white shadow-lg rounded-lg my-5 w-60 shrink-0">
                                <div class="flex justify-center md:justify-start -mt-7">
                                    <div class="w-10 h-10 object-cover rounded-full border-2 border-brownish-red-dark bg-brownish-red text-white text-center"><span class="">{{i}}</span></div>
                                </div>
                                <div class="py-2 flex justify-between">
                                    <h2 class="text-gray-800 text-base font-semibold pr-8">Spanish</h2>
                                    <p class="text-sm font-semibold text-brownish-red-darker mt-1 ">Pl00001</p>
                                </div>
                                <div class="flex justify-end py-2">
                                    <button class="border hover:shadow-sm hover:border-2 shadow border-green-400 px-3 py-0.5 rounded-full text-xs font-semibold text-green-400 "  @click="openModal">Rate Restaurant</button>
                                </div>
                            </div>
                        </div>
                        <div  class="h-full w-1/6 flex justify-center items-center py-14">
                        </div>
                    </div>
                </div>
                <!-- Modal -->
                <Modal :width="'w-1/3'" v-if="this.modal" @close="closeModal">
                    <template v-slot:header>
                        <h3 class="">Review Restaurant</h3>
                    </template>
                    <template v-slot:body>
                        <div class="py-2 pb-4">
                            <p class="text-sm text-center font-semibold">Food Rating</p>
                            <div class="flex justify-center">
                                <svg class="h-8 w-8 fill-current text-yellow-400 pr-2" fill="#000000" version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" 
                                        width="800px" height="800px" viewBox="0 0 36.09 36.09" xml:space="preserve"
                                        >
                                    <g>
                                        <path d="M36.042,13.909c-0.123-0.377-0.456-0.646-0.85-0.688l-11.549-1.172L18.96,1.43c-0.16-0.36-0.519-0.596-0.915-0.596
                                            s-0.755,0.234-0.915,0.598L12.446,12.05L0.899,13.221c-0.394,0.04-0.728,0.312-0.85,0.688c-0.123,0.377-0.011,0.791,0.285,1.055
                                            l8.652,7.738L6.533,34.045c-0.083,0.387,0.069,0.787,0.39,1.02c0.175,0.127,0.381,0.191,0.588,0.191
                                            c0.173,0,0.347-0.045,0.503-0.137l10.032-5.84l10.03,5.84c0.342,0.197,0.77,0.178,1.091-0.059c0.32-0.229,0.474-0.633,0.391-1.02
                                            l-2.453-11.344l8.653-7.737C36.052,14.699,36.165,14.285,36.042,13.909z M25.336,21.598c-0.268,0.24-0.387,0.605-0.311,0.957
                                            l2.097,9.695l-8.574-4.99c-0.311-0.182-0.695-0.182-1.006,0l-8.576,4.99l2.097-9.695c0.076-0.352-0.043-0.717-0.311-0.957
                                            l-7.396-6.613l9.87-1.002c0.358-0.035,0.668-0.264,0.814-0.592l4.004-9.077l4.003,9.077c0.146,0.328,0.456,0.557,0.814,0.592
                                            l9.87,1.002L25.336,21.598z"/>
                                    </g>
                                </svg>
                                <svg class="h-8 w-8 fill-current text-yellow-400 pr-2" fill="#000000" version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" 
                                        width="800px" height="800px" viewBox="0 0 36.09 36.09" xml:space="preserve"
                                        >
                                    <g>
                                        <path d="M36.042,13.909c-0.123-0.377-0.456-0.646-0.85-0.688l-11.549-1.172L18.96,1.43c-0.16-0.36-0.519-0.596-0.915-0.596
                                            s-0.755,0.234-0.915,0.598L12.446,12.05L0.899,13.221c-0.394,0.04-0.728,0.312-0.85,0.688c-0.123,0.377-0.011,0.791,0.285,1.055
                                            l8.652,7.738L6.533,34.045c-0.083,0.387,0.069,0.787,0.39,1.02c0.175,0.127,0.381,0.191,0.588,0.191
                                            c0.173,0,0.347-0.045,0.503-0.137l10.032-5.84l10.03,5.84c0.342,0.197,0.77,0.178,1.091-0.059c0.32-0.229,0.474-0.633,0.391-1.02
                                            l-2.453-11.344l8.653-7.737C36.052,14.699,36.165,14.285,36.042,13.909z M25.336,21.598c-0.268,0.24-0.387,0.605-0.311,0.957
                                            l2.097,9.695l-8.574-4.99c-0.311-0.182-0.695-0.182-1.006,0l-8.576,4.99l2.097-9.695c0.076-0.352-0.043-0.717-0.311-0.957
                                            l-7.396-6.613l9.87-1.002c0.358-0.035,0.668-0.264,0.814-0.592l4.004-9.077l4.003,9.077c0.146,0.328,0.456,0.557,0.814,0.592
                                            l9.87,1.002L25.336,21.598z"/>
                                    </g>
                                </svg>
                                <svg class="h-8 w-8 fill-current pr-2" fill="#000000" version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" 
                                        width="800px" height="800px" viewBox="0 0 36.09 36.09" xml:space="preserve"
                                        >
                                    <g>
                                        <path d="M36.042,13.909c-0.123-0.377-0.456-0.646-0.85-0.688l-11.549-1.172L18.96,1.43c-0.16-0.36-0.519-0.596-0.915-0.596
                                            s-0.755,0.234-0.915,0.598L12.446,12.05L0.899,13.221c-0.394,0.04-0.728,0.312-0.85,0.688c-0.123,0.377-0.011,0.791,0.285,1.055
                                            l8.652,7.738L6.533,34.045c-0.083,0.387,0.069,0.787,0.39,1.02c0.175,0.127,0.381,0.191,0.588,0.191
                                            c0.173,0,0.347-0.045,0.503-0.137l10.032-5.84l10.03,5.84c0.342,0.197,0.77,0.178,1.091-0.059c0.32-0.229,0.474-0.633,0.391-1.02
                                            l-2.453-11.344l8.653-7.737C36.052,14.699,36.165,14.285,36.042,13.909z M25.336,21.598c-0.268,0.24-0.387,0.605-0.311,0.957
                                            l2.097,9.695l-8.574-4.99c-0.311-0.182-0.695-0.182-1.006,0l-8.576,4.99l2.097-9.695c0.076-0.352-0.043-0.717-0.311-0.957
                                            l-7.396-6.613l9.87-1.002c0.358-0.035,0.668-0.264,0.814-0.592l4.004-9.077l4.003,9.077c0.146,0.328,0.456,0.557,0.814,0.592
                                            l9.87,1.002L25.336,21.598z"/>
                                    </g>
                                </svg>
                                <svg class="h-8 w-8 fill-current pr-2" fill="#000000" version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" 
                                        width="800px" height="800px" viewBox="0 0 36.09 36.09" xml:space="preserve"
                                        >
                                    <g>
                                        <path d="M36.042,13.909c-0.123-0.377-0.456-0.646-0.85-0.688l-11.549-1.172L18.96,1.43c-0.16-0.36-0.519-0.596-0.915-0.596
                                            s-0.755,0.234-0.915,0.598L12.446,12.05L0.899,13.221c-0.394,0.04-0.728,0.312-0.85,0.688c-0.123,0.377-0.011,0.791,0.285,1.055
                                            l8.652,7.738L6.533,34.045c-0.083,0.387,0.069,0.787,0.39,1.02c0.175,0.127,0.381,0.191,0.588,0.191
                                            c0.173,0,0.347-0.045,0.503-0.137l10.032-5.84l10.03,5.84c0.342,0.197,0.77,0.178,1.091-0.059c0.32-0.229,0.474-0.633,0.391-1.02
                                            l-2.453-11.344l8.653-7.737C36.052,14.699,36.165,14.285,36.042,13.909z M25.336,21.598c-0.268,0.24-0.387,0.605-0.311,0.957
                                            l2.097,9.695l-8.574-4.99c-0.311-0.182-0.695-0.182-1.006,0l-8.576,4.99l2.097-9.695c0.076-0.352-0.043-0.717-0.311-0.957
                                            l-7.396-6.613l9.87-1.002c0.358-0.035,0.668-0.264,0.814-0.592l4.004-9.077l4.003,9.077c0.146,0.328,0.456,0.557,0.814,0.592
                                            l9.87,1.002L25.336,21.598z"/>
                                    </g>
                                </svg>
                                <svg class="h-8 w-8 fill-current pr-2" fill="#000000" version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" 
                                        width="800px" height="800px" viewBox="0 0 36.09 36.09" xml:space="preserve"
                                        >
                                    <g>
                                        <path d="M36.042,13.909c-0.123-0.377-0.456-0.646-0.85-0.688l-11.549-1.172L18.96,1.43c-0.16-0.36-0.519-0.596-0.915-0.596
                                            s-0.755,0.234-0.915,0.598L12.446,12.05L0.899,13.221c-0.394,0.04-0.728,0.312-0.85,0.688c-0.123,0.377-0.011,0.791,0.285,1.055
                                            l8.652,7.738L6.533,34.045c-0.083,0.387,0.069,0.787,0.39,1.02c0.175,0.127,0.381,0.191,0.588,0.191
                                            c0.173,0,0.347-0.045,0.503-0.137l10.032-5.84l10.03,5.84c0.342,0.197,0.77,0.178,1.091-0.059c0.32-0.229,0.474-0.633,0.391-1.02
                                            l-2.453-11.344l8.653-7.737C36.052,14.699,36.165,14.285,36.042,13.909z M25.336,21.598c-0.268,0.24-0.387,0.605-0.311,0.957
                                            l2.097,9.695l-8.574-4.99c-0.311-0.182-0.695-0.182-1.006,0l-8.576,4.99l2.097-9.695c0.076-0.352-0.043-0.717-0.311-0.957
                                            l-7.396-6.613l9.87-1.002c0.358-0.035,0.668-0.264,0.814-0.592l4.004-9.077l4.003,9.077c0.146,0.328,0.456,0.557,0.814,0.592
                                            l9.87,1.002L25.336,21.598z"/>
                                    </g>
                                </svg>
                            </div>
                        </div>
                        <div class="py-2 pb-4">
                            <p class="text-sm text-center font-semibold">Service Rating</p>
                            <div class="flex justify-center">
                                <svg class="h-8 w-8 fill-current text-yellow-400 pr-2" fill="#000000" version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" 
                                        width="800px" height="800px" viewBox="0 0 36.09 36.09" xml:space="preserve"
                                        >
                                    <g>
                                        <path d="M36.042,13.909c-0.123-0.377-0.456-0.646-0.85-0.688l-11.549-1.172L18.96,1.43c-0.16-0.36-0.519-0.596-0.915-0.596
                                            s-0.755,0.234-0.915,0.598L12.446,12.05L0.899,13.221c-0.394,0.04-0.728,0.312-0.85,0.688c-0.123,0.377-0.011,0.791,0.285,1.055
                                            l8.652,7.738L6.533,34.045c-0.083,0.387,0.069,0.787,0.39,1.02c0.175,0.127,0.381,0.191,0.588,0.191
                                            c0.173,0,0.347-0.045,0.503-0.137l10.032-5.84l10.03,5.84c0.342,0.197,0.77,0.178,1.091-0.059c0.32-0.229,0.474-0.633,0.391-1.02
                                            l-2.453-11.344l8.653-7.737C36.052,14.699,36.165,14.285,36.042,13.909z M25.336,21.598c-0.268,0.24-0.387,0.605-0.311,0.957
                                            l2.097,9.695l-8.574-4.99c-0.311-0.182-0.695-0.182-1.006,0l-8.576,4.99l2.097-9.695c0.076-0.352-0.043-0.717-0.311-0.957
                                            l-7.396-6.613l9.87-1.002c0.358-0.035,0.668-0.264,0.814-0.592l4.004-9.077l4.003,9.077c0.146,0.328,0.456,0.557,0.814,0.592
                                            l9.87,1.002L25.336,21.598z"/>
                                    </g>
                                </svg>
                                <svg class="h-8 w-8 fill-current text-yellow-400 pr-2" fill="#000000" version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" 
                                        width="800px" height="800px" viewBox="0 0 36.09 36.09" xml:space="preserve"
                                        >
                                    <g>
                                        <path d="M36.042,13.909c-0.123-0.377-0.456-0.646-0.85-0.688l-11.549-1.172L18.96,1.43c-0.16-0.36-0.519-0.596-0.915-0.596
                                            s-0.755,0.234-0.915,0.598L12.446,12.05L0.899,13.221c-0.394,0.04-0.728,0.312-0.85,0.688c-0.123,0.377-0.011,0.791,0.285,1.055
                                            l8.652,7.738L6.533,34.045c-0.083,0.387,0.069,0.787,0.39,1.02c0.175,0.127,0.381,0.191,0.588,0.191
                                            c0.173,0,0.347-0.045,0.503-0.137l10.032-5.84l10.03,5.84c0.342,0.197,0.77,0.178,1.091-0.059c0.32-0.229,0.474-0.633,0.391-1.02
                                            l-2.453-11.344l8.653-7.737C36.052,14.699,36.165,14.285,36.042,13.909z M25.336,21.598c-0.268,0.24-0.387,0.605-0.311,0.957
                                            l2.097,9.695l-8.574-4.99c-0.311-0.182-0.695-0.182-1.006,0l-8.576,4.99l2.097-9.695c0.076-0.352-0.043-0.717-0.311-0.957
                                            l-7.396-6.613l9.87-1.002c0.358-0.035,0.668-0.264,0.814-0.592l4.004-9.077l4.003,9.077c0.146,0.328,0.456,0.557,0.814,0.592
                                            l9.87,1.002L25.336,21.598z"/>
                                    </g>
                                </svg>
                                <svg class="h-8 w-8 fill-current pr-2" fill="#000000" version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" 
                                        width="800px" height="800px" viewBox="0 0 36.09 36.09" xml:space="preserve"
                                        >
                                    <g>
                                        <path d="M36.042,13.909c-0.123-0.377-0.456-0.646-0.85-0.688l-11.549-1.172L18.96,1.43c-0.16-0.36-0.519-0.596-0.915-0.596
                                            s-0.755,0.234-0.915,0.598L12.446,12.05L0.899,13.221c-0.394,0.04-0.728,0.312-0.85,0.688c-0.123,0.377-0.011,0.791,0.285,1.055
                                            l8.652,7.738L6.533,34.045c-0.083,0.387,0.069,0.787,0.39,1.02c0.175,0.127,0.381,0.191,0.588,0.191
                                            c0.173,0,0.347-0.045,0.503-0.137l10.032-5.84l10.03,5.84c0.342,0.197,0.77,0.178,1.091-0.059c0.32-0.229,0.474-0.633,0.391-1.02
                                            l-2.453-11.344l8.653-7.737C36.052,14.699,36.165,14.285,36.042,13.909z M25.336,21.598c-0.268,0.24-0.387,0.605-0.311,0.957
                                            l2.097,9.695l-8.574-4.99c-0.311-0.182-0.695-0.182-1.006,0l-8.576,4.99l2.097-9.695c0.076-0.352-0.043-0.717-0.311-0.957
                                            l-7.396-6.613l9.87-1.002c0.358-0.035,0.668-0.264,0.814-0.592l4.004-9.077l4.003,9.077c0.146,0.328,0.456,0.557,0.814,0.592
                                            l9.87,1.002L25.336,21.598z"/>
                                    </g>
                                </svg>
                                <svg class="h-8 w-8 fill-current pr-2" fill="#000000" version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" 
                                        width="800px" height="800px" viewBox="0 0 36.09 36.09" xml:space="preserve"
                                        >
                                    <g>
                                        <path d="M36.042,13.909c-0.123-0.377-0.456-0.646-0.85-0.688l-11.549-1.172L18.96,1.43c-0.16-0.36-0.519-0.596-0.915-0.596
                                            s-0.755,0.234-0.915,0.598L12.446,12.05L0.899,13.221c-0.394,0.04-0.728,0.312-0.85,0.688c-0.123,0.377-0.011,0.791,0.285,1.055
                                            l8.652,7.738L6.533,34.045c-0.083,0.387,0.069,0.787,0.39,1.02c0.175,0.127,0.381,0.191,0.588,0.191
                                            c0.173,0,0.347-0.045,0.503-0.137l10.032-5.84l10.03,5.84c0.342,0.197,0.77,0.178,1.091-0.059c0.32-0.229,0.474-0.633,0.391-1.02
                                            l-2.453-11.344l8.653-7.737C36.052,14.699,36.165,14.285,36.042,13.909z M25.336,21.598c-0.268,0.24-0.387,0.605-0.311,0.957
                                            l2.097,9.695l-8.574-4.99c-0.311-0.182-0.695-0.182-1.006,0l-8.576,4.99l2.097-9.695c0.076-0.352-0.043-0.717-0.311-0.957
                                            l-7.396-6.613l9.87-1.002c0.358-0.035,0.668-0.264,0.814-0.592l4.004-9.077l4.003,9.077c0.146,0.328,0.456,0.557,0.814,0.592
                                            l9.87,1.002L25.336,21.598z"/>
                                    </g>
                                </svg>
                                <svg class="h-8 w-8 fill-current pr-2" fill="#000000" version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" 
                                        width="800px" height="800px" viewBox="0 0 36.09 36.09" xml:space="preserve"
                                        >
                                    <g>
                                        <path d="M36.042,13.909c-0.123-0.377-0.456-0.646-0.85-0.688l-11.549-1.172L18.96,1.43c-0.16-0.36-0.519-0.596-0.915-0.596
                                            s-0.755,0.234-0.915,0.598L12.446,12.05L0.899,13.221c-0.394,0.04-0.728,0.312-0.85,0.688c-0.123,0.377-0.011,0.791,0.285,1.055
                                            l8.652,7.738L6.533,34.045c-0.083,0.387,0.069,0.787,0.39,1.02c0.175,0.127,0.381,0.191,0.588,0.191
                                            c0.173,0,0.347-0.045,0.503-0.137l10.032-5.84l10.03,5.84c0.342,0.197,0.77,0.178,1.091-0.059c0.32-0.229,0.474-0.633,0.391-1.02
                                            l-2.453-11.344l8.653-7.737C36.052,14.699,36.165,14.285,36.042,13.909z M25.336,21.598c-0.268,0.24-0.387,0.605-0.311,0.957
                                            l2.097,9.695l-8.574-4.99c-0.311-0.182-0.695-0.182-1.006,0l-8.576,4.99l2.097-9.695c0.076-0.352-0.043-0.717-0.311-0.957
                                            l-7.396-6.613l9.87-1.002c0.358-0.035,0.668-0.264,0.814-0.592l4.004-9.077l4.003,9.077c0.146,0.328,0.456,0.557,0.814,0.592
                                            l9.87,1.002L25.336,21.598z"/>
                                    </g>
                                </svg>
                            </div>
                        </div>
                    </template>
                    <template  v-slot:footer>
                        <div class="flex justify-center py-6">
                            <button class=" border-2 rounded px-2 py-1 border-brownish-red-dark bg-white">
                                <span class="text-sm font-semibold text-gray-600 hover:text-brownish-red-dark">Submit</span> 
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