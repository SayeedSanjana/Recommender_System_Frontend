<script>
export default{
    methods:{
        logout(){
            this.$router.push('/')
        }
    }
}
</script>
<template>
    <nav class="flex bg-red-50 bg-opacity-40 items-center relative justify-between  px-4 py-2 w-full border-b-4 border-opacity-30 shadow-sm border-brownish-light">
  <div class="flex">
    <img src="https://img.icons8.com/glyph-neue/64/000000/restaurant-building.png"/>
    <p class="font-mono px-2 font-semibold text-gray-600 py-6 text-lg">Feast-Hub</p>
 </div>

  <div class="flex font-mono">
    <img class="object-cover w-10 h-10 rounded-full" src="https://images.unsplash.com/photo-1531746020798-e6953c6e8e04?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=764&h=764&q=100" alt="">
     <div class="px-1.5">
        <div class="text-sm"><span class="font-semibold text-gray-600">Welcome, </span><span class=" font-bold text-base text-gray-700">User1234</span></div>
        <div class="text-xs font-medium text-gray-700">18th March, 2023</div>
     </div>

     <div class="flex px-2.5 py-0.5 border-l-2 border-brownish-red-dark border-spacing-6 border-opacity-40">
        <span>
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-brownish-red" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 16l4-4m0 0l-4-4m4 4H7m6 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1" />
            </svg>
        </span>
        <span @click="logout" class="text-brownish-red-dark hover:text-brownish-red hover:underline cursor-pointer font-semibold"> Logout </span>
     </div>
  </div>
</nav>
</template>
<style scoped>

</style>