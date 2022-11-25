<template>
    <section class="bg-gray-50 dark:bg-gray-900">
        <div class="flex flex-col items-center justify-center px-6 py-8 mx-auto py-auto md:h-screen lg:py-2 mt-4">
            <div class="w-full bg-white rounded-lg shadow dark:border md:mt-0 sm:max-w-md xl:p-0 dark:bg-gray-800 dark:border-gray-700">
                <div class="p-6 space-y-4 md:space-y-6 sm:p-8">
                    <h1 class="text-xl font-bold leading-tight tracking-tight text-gray-900 md:text-2xl dark:text-white">
                        Faça login com a sua conta SCM
                    </h1>

                    <div class="flex p-4 mb-4 text-sm text-red-700 bg-red-100 rounded-lg dark:bg-red-200 dark:text-red-800" role="alert" v-if="responseInvalid">
                            <svg aria-hidden="true" class="flex-shrink-0 inline w-5 h-5 mr-3" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2v-3a1 1 0 00-1-1H9z" clip-rule="evenodd"></path></svg>
                        <span class="sr-only">Info</span>
                        <div>
                            <span class="font-medium">Ops!</span> Email e/ou senha inválidos.
                        </div>
                    </div>

                    <form class="space-y-4 md:space-y-6" action="#" @submit.stop.prevent="login">
                        <div>
                            <label for="email" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Email</label>
                            <input 
                                type="email" 
                                class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="nome@email.com"
                                v-model="email"
                            >  
                        </div>
                        <div>
                            <label for="password" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Senha</label>
                            <input 
                                type="password" 
                                placeholder="••••••••" 
                                class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                                v-model="password"
                            >
                        </div>
                        <div class="flex items-center justify-between">
                            <div class="flex items-start">
                                <div class="flex items-center h-5">
                                    <input id="remember" aria-describedby="remember" type="checkbox" class="w-4 h-4 border border-gray-300 rounded bg-gray-50 focus:ring-3 focus:ring-primary-300 dark:bg-gray-700 dark:border-gray-600 dark:focus:ring-primary-600 dark:ring-offset-gray-800">
                                </div>
                                <div class="ml-3 text-sm">
                                    <label for="remember" class="text-gray-500 dark:text-gray-300">Lembrar-me</label>
                                </div>
                            </div>
                            <a href="#" class="text-sm font-medium text-primary-600 hover:underline dark:text-primary-500">Esqueci minha senha</a>
                        </div>
                        <button
                            :disabled="setSpinner" 
                            type="submit" 
                            class="w-full text-white bg-primary-600 hover:bg-primary-700 focus:ring-4 focus:outline-none focus:ring-primary-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-primary-600 dark:hover:bg-primary-700 dark:focus:ring-primary-800"
                            >
                            <SpinnerBox v-if="setSpinner"/>  
                            <span v-else>Login</span>
                        </button>
                        <p class="text-sm font-light text-gray-500 dark:text-gray-400">
                            Não possui uma conta ? <a href="/register" class="font-medium text-primary-600 hover:underline dark:text-primary-500">Registrar</a>
                        </p>
                    </form>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
import axios from 'axios'
import Cookie from 'js-cookie'
import SpinnerBox from '@/components/SpinnerBox.vue'

    export default{
        name: 'login-user',

        components: {
            SpinnerBox,
        },

        data() {
            return {
                email: '',
                password: '',
                responseInvalid: false,
                setSpinner: false
            }
        },

        methods: {
            login() {
                const payload = {
                    email: this.email,
                    password: this.password
                }

                this.resetResponse()
                this.setSpinner = true

                axios.post('v1/login', payload).then((response) => {
                    const token = `${response.data.token_type} ${response.data.access_token}` 
                    Cookie.set('_todoList_token', token, {expires: 30})

                    this.$store.commit('user/STORE_USER', response.data.data)

                }).catch(() => {
                    this.responseInvalid = true
                    this.setSpinner = false
                })
            },
            
            resetResponse () {
                this.responseInvalid = false
            }
        },
    }

</script>