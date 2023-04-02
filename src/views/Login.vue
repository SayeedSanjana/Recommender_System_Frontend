<script >
import axios from "axios";
import swal from 'sweetalert';
import useValidate from '@vuelidate/core';
import {required,minLength,email} from '@vuelidate/validators';
export default{
	data(){
		return{
			formData: {
                idenfitier: '',
                password: '',
            },
		}
	},
	setup(){
            return{
            v$: useValidate(),
            }
        },
	validations() {
		return {
			formData: {
				idenfitier: {
					required,
				},
				password: {
					required,
					minLength: minLength(5)
				},
			}
		}
	},
	methods:{
		async loginUser() {
			this.v$.$touch()
			if (!this.v$.$error) {
				await axios.post(
					'http://localhost:8000/api/users/login', this.formData, )
					.then((response) => {
						if (response.data.data !== null) {
							localStorage.setItem("token", response.data.data)
						}
						this.$router.push('/home');
					})
					.catch((error) => {
						swal({
							title: "Invalid Information",
							text: "Invalid Email or Password",
							icon: "error",
							buttons: true,
						})
					})
			}
		}
    },
}

</script>

<template>
  
   <!-- component -->
<div class="font-mono bg-red-50 p-1 h-screen ">
		<!-- Container -->
		<div class="container mx-auto">
			<div class="flex justify-center px-6 my-12">
				<!-- Row -->
				<div class="w-full xl:w-3/4 lg:w-11/12 flex">
					<!-- Col -->
					<div
						class="w-full h-auto bg-gray-400 hidden lg:block lg:w-1/2 bg-cover rounded-l-lg"
						style="background-image: url('https://i0.wp.com/sincerelytori.com/wp-content/uploads/2022/09/Alexandra-Onosa-@bakingmydays-1.jpg?resize=960%2C1200&ssl=1')"></div>
					<!-- Col -->
					<div class="w-full lg:w-1/2 bg-white p-5 rounded-lg lg:rounded-l-none">
						<h3 class="pt-4 text-2xl font-semibold text-center">Welcome Back!!</h3>
						<form class="px-8 pt-6 pb-8 mb-4 bg-white rounded"  @submit.prevent="loginUser">
							<div class="mb-4">
								<label class="block mb-2 text-sm font-bold text-gray-700" for="username">
									Username
								</label>
								<input
								    v-model.trim="formData.idenfitier"  @blur="v$.formData.idenfitier.$touch()" 
									class="w-full px-3 py-2 text-sm leading-tight text-gray-700 border rounded shadow appearance-none focus:outline-none focus:shadow-outline"
									id="username"
									type="text"
									placeholder="Username"
								/>
								<small class="text-red-500 flex justify-start text-xs" v-if="v$.formData.idenfitier.$error">{{v$.formData.idenfitier.$errors[0].$message}}</small>
							</div>
							<div class="mb-4">
								<label class="block mb-2 text-sm font-bold text-gray-700" for="password">
									Password
								</label>
								<input
								    v-model="formData.password" @blur="v$.formData.password.$touch()"
									class="w-full px-3 py-2 text-sm leading-tight text-gray-700 border rounded shadow appearance-none focus:outline-none focus:shadow-outline"
									id="password"
									type="password"
									placeholder="******************"
								/>
								<small class="text-red-500 flex justify-start text-xs" v-if="v$.formData.password.$error">{{v$.formData.password.$errors[0].$message}}</small>
								<!-- <p class="text-xs italic text-red-500">Please choose a password.</p> -->
							</div>
							<div class="mb-4">
								<input class="mr-2 leading-tight text-brownish-red" type="checkbox" id="checkbox_id" />
								<label class="text-sm text-brownish-red-dark font-medium" for="checkbox_id">
									Remember Me
								</label>
							</div>
							<div class="mb-6 text-center">
								<button class="w-full px-4 py-2 font-bold text-white bg-brownish-red rounded-full hover:bg-brownish-red-dark focus:outline-none focus:shadow-outline"
									type="submit">
									Login
								</button>
							</div>
							<hr class="mb-6 border-t" />
							<div class="text-center">
								<p class="inline-block text-sm  align-baseline ">
									<span class="cursor-none text-gray-600"> Don't have an account yet?</span>
									<a class="cursor-pointer p-1  font-semibold text-brownish-red-dark hover:text-brownish-red" href="#">
										<router-link to="/sign-up">Sign Up</router-link>
									</a>
								</p>
							</div>
						</form>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>
<style scoped>
</style>
