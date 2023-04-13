<script>
import axios from "axios";
import swal from 'sweetalert';
import useValidate from '@vuelidate/core';
import {required,minLength,maxLength,numeric,email,helpers} from '@vuelidate/validators';
export default{
	data(){
		return{
			formData: {
                username: '',
                email: '',
				password:'',
				phone_number:''
            },
		}
	},
	setup(){
            return{
            v$: useValidate(),
            }
        },
		validations() {
			const pattern = helpers.regex(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/);
			//const nospecial = helpers.regex(/^[A-Za-z\s]+$/);
		return {
			formData: {
				username: {
					required,
					minLength: minLength(3),
  					maxLength: maxLength(255),
				},
				email:{
                    required,
					email
				},
				password: {
					required,
					minLength: minLength(5),
					pattern: helpers.withMessage("Should include 0-9,A-Z, a-z and special characters like '@,#,$,*'", pattern)
				},
				phone_number:{
					required,
					numeric,
					minLength: minLength(10),
  					maxLength: maxLength(14)
				}
			}
		}
	},
	methods:{
		async registerUser(){
			this.v$.$touch()
			if (!this.v$.$error) {
				await axios.post(
					'http://localhost:8000/api/users/signup', this.formData, )
					.then((response) => {
						swal({
							title: "Success",
							text: "User created Successfully!",
							icon: "success",
							timer: 1000,
							buttons: false
						})
						// if (response.data.data !== null) {
						// 	localStorage.setItem("token", response.data.data)
						// }
						this.$router.push('/');
					})
					.catch((error) => {
						swal({
							title: "Invalid Information",
							text: "Invalid Username, Email, Password or Phone ",
							icon: "error",
							buttons: true,
						})
					})
			}
		}
	}
}


</script>

<template>
  
   <!-- component -->
<div class="font-mono bg-red-50 p-1 h-screen ">
		<!-- Container -->
		<div class="container mx-auto">
			<div class="flex justify-center px-6 my-6">
				<!-- Row -->
				<div class="w-full xl:w-3/4 lg:w-11/12 flex">
				
					<!-- Col -->
					<div class="w-full lg:w-1/2 bg-white p-2 rounded-lg lg:rounded-l-none">
						<h3 class="pt-2 text-2xl font-semibold text-center">Welcome!!</h3>
						<form class="px-8 pt-3 pb-6 mb-4 bg-white rounded" @submit.prevent="registerUser">
							<div class="mb-4">
								<label class="block mb-2 text-sm font-bold text-gray-700" for="username">
									Username
								</label>
								<input v-model="formData.username"  @blur="v$.formData.username.$touch()"
									class="w-full px-3 py-2 text-sm leading-tight text-gray-700 border rounded shadow appearance-none focus:outline-none focus:shadow-outline"
									id="username"
									type="text"
									placeholder="Username"/>
								<small class="text-red-400 flex justify-start text-xs" v-if="v$.formData.username.$error">{{v$.formData.username.$errors[0].$message}}</small>
							</div>
							<div class="mb-4">
								<label class="block mb-2 text-sm font-bold text-gray-700" for="email">
									Email
								</label>
								<input v-model="formData.email"  @blur="v$.formData.email.$touch()"
									class="w-full px-3 py-2 text-sm leading-tight text-gray-700 border rounded shadow appearance-none focus:outline-none focus:shadow-outline"
									id="email"
									type="email"
									placeholder="Email"/>
								<small class="text-red-400 flex justify-start text-xs" v-if="v$.formData.email.$error">{{v$.formData.email.$errors[0].$message}}</small>
							</div>
							<div class="mb-4">
								<label class="block mb-2 text-sm font-bold text-gray-700" for="password">
									Password
								</label>
								<input v-model="formData.password"  @blur="v$.formData.password.$touch()"
									class="w-full px-3 py-2 text-sm leading-tight text-gray-700 border rounded shadow appearance-none focus:outline-none focus:shadow-outline"
									id="password"
									type="password"
									placeholder="******************"
								/>
								<small class="text-red-400 flex justify-start text-xs" v-if="v$.formData.password.$error">{{v$.formData.password.$errors[0].$message}}</small>

								<!-- <p class="text-xs italic text-red-500">Please choose a password.</p> -->
							</div>
							<div class="mb-4">
								<label class="block mb-2 text-sm font-bold text-gray-700" for="phone">
									Phone
								</label>
								<input v-model="formData.phone_number"  @blur="v$.formData.phone_number.$touch()"
									class="w-full px-3 py-2 text-sm leading-tight text-gray-700 border rounded shadow appearance-none focus:outline-none focus:shadow-outline"
									id="phone"
									type="phone"
									placeholder="Phone"
								/>
								<small class="text-red-400 flex justify-start text-xs" v-if="v$.formData.phone_number.$error">{{v$.formData.phone_number.$errors[0].$message}}</small>
								<!-- <p class="text-xs italic text-red-500">Please choose a password.</p> -->
							</div>
							<div class="mb-4">
								<input class="mr-2 leading-tight text-brownish-red" type="checkbox" id="checkbox_id" />
								<label class="text-sm text-brownish-red-dark font-medium" for="checkbox_id">
									Remember Me
								</label>
							</div>
							<div class="mb-6 text-center">
								<button
									class="w-full px-4 py-2 font-bold text-white bg-brownish-red rounded-full hover:bg-brownish-red-dark focus:outline-none focus:shadow-outline"
									type="submit">
									Sign Up
								</button>
							</div>
							<hr class="mb-6 border-t" />
							<div class="text-center">
								<p class="inline-block text-sm  align-baseline ">
									<span class="cursor-none text-gray-600">Already have an account?</span>
									<a class="cursor-pointer p-1  font-semibold text-brownish-red-dark hover:text-brownish-red" href="#">
										<router-link to="/">Login</router-link>
									</a>
								</p>
							</div>
						</form>
					</div>
						<!-- Col -->
					<div class="w-full h-auto bg-gray-400 hidden lg:block max-h-screen lg:w-1/2 bg-cover rounded-l-lg" style="background-image: url('https://cdn.shopify.com/s/files/1/0555/0073/4619/files/Kera02-El-Cielo_2000x.jpg?v=1650695707')"></div>
				</div>
			</div>
		</div>
	</div>
</template>
<style scoped>

</style>