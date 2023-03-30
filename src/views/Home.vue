<script>
import Nav from "../components/nav.vue";
import Jumbotron from "../components/jumbotron.vue";
export default{
    components: {
		Nav,
        Jumbotron
	},

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


<section class="antialiased bg-gray-100 text-gray-600  px-4 py-4 font-mono" x-data="app">
    <div class="flex flex-col lg:w-1/2">
        <!-- Table -->
        <div class="w-full max-w-2xl mx-auto bg-white shadow-lg rounded-sm border border-gray-200">
            <header class="px-5 py-4 border-b border-gray-100">
                <div class="font-semibold text-gray-700"> Revisited restaurant history: </div>
            </header>

            <div class="overflow-x-auto p-3">
                <table class="table-auto w-full">
                    <thead class="text-xs font-semibold uppercase text-gray-700 bg-brownish-red-light bg-opacity-30">
                        <tr>
                            <th></th>
                            <th class="p-2">
                                <div class="font-semibold text-left">Cuisine Name</div>
                            </th>
                            <th class="p-2">
                                <div class="font-semibold text-left">Place ID</div>
                            </th>
                            <th class="p-2">
                                <div class="font-semibold text-left">Overall Rating</div>
                            </th>
                            <th class="p-2">
                                <div class="font-semibold text-center"></div>
                            </th>
                        </tr>
                    </thead>

                    <tbody class="text-sm divide-y divide-gray-100">
                        <!-- record 1 -->
                        <tr v-for="x in 5" :key="x">
                            <td class="p-2">
                                <!-- <input type="checkbox" class="w-5 h-5" value="id-1"
                                    @click="toggleCheckbox($el, 2890.66)" /> -->
                            </td>
                            <td class="p-2">
                                <div class="font-medium text-gray-800">
                                    Spanish
                                </div>
                            </td>
                            <td class="p-2">
                                <div class="text-left">P10001</div>
                            </td>
                            <td class="p-2">
                                <div class="text-left font-medium text-base text-green-500">
                                    02
                                </div>
                            </td>
                            <td class="p-2">
                                <div class="flex justify-center">
                                    <button>
                                        <svg class="w-8 h-8 hover:text-red-600 rounded-full hover:bg-gray-100 p-1"
                                            fill="none" stroke="currentColor" viewBox="0 0 24 24"
                                            xmlns="http://www.w3.org/2000/svg">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                                d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16">
                                            </path>
                                        </svg>
                                    </button>
                                </div>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
    <div class="lg:w-1/2 flex flex-col">

    </div>
</section>

        </div>
       
    </div>
  
</template>

<style scoped>

</style>