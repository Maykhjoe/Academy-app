<template>

    <div class="container-fluid mt-5">
        <div class="row justify-content-center">
            <div class="col-md-6">
                <div class="card border-0 rounded shadow">
                    <div class="card-body">
                        <h4>REGISTER</h4>
                        <hr>
                        <form @submit.prevent="register">
                            <div class="row">
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label>First Name</label>
                                        <input type="text" v-model="user.first_name" class="form-control"
                                            placeholder="First Name">
                                    </div>
                                    <div v-if="validation.first_name" class="mt-2 alert alert-danger">
                                        {{ validation.first_name[0] }}
                                    </div>
                                
                                    <div class="form-group">
                                        <label>Last Name</label>
                                        <input type="text" v-model="user.last_name" class="form-control"
                                            placeholder="Last Name">
                                    </div>
                                    <div v-if="validation.last_name" class="mt-2 alert alert-danger">
                                        {{ validation.last_name[0] }}
                                    </div>
                                    <div class="form-group">
                                        <label>Organization Name</label>
                                        <input type="text" v-model="user.organization_name" class="form-control"
                                            placeholder="Organization Name">
                                    </div>
                                    <div v-if="validation.organization_name" class="mt-2 alert alert-danger">
                                        {{ validation.organization_name[0] }}
                                    </div>
                                </div>
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label>Email address</label>
                                        <input type="email" v-model="user.email" class="form-control"
                                            placeholder="Email Address">
                                    </div>
                                    <div v-if="validation.email" class="mt-2 alert alert-danger">
                                        {{ validation.email[0] }}
                                    </div>
                                </div>
                            </div>

                            <div class="row">
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label>Password</label>
                                        <input type="password" v-model="user.password" class="form-control"
                                            placeholder="Password">
                                    </div>
                                    <div v-if="validation.password" class="mt-2 alert alert-danger">
                                        {{ validation.password[0] }}
                                    </div>
                                </div>
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label>Konfirmasi Password</label>
                                        <input type="password" v-model="user.password_confirmation" class="form-control"
                                            placeholder="Konfirmasi Password">
                                    </div>
                                </div>
                            </div>
                            <button type="submit" class="btn btn-primary btn-block">REGISTER</button>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>

</template>

<script>
    import { reactive, ref } from 'vue'
    import { useRouter } from 'vue-router'
    import axios from 'axios'

    export default {


        setup() {

            //inisialisasi vue router on Composition API
            const router = useRouter()

            //state user
            const user = reactive({
                first_name: '',
                last_name: '',
                organization_name: '',
                email: '',
                password: '',
                password_confirmation: ''
            })

            //state validation
            const validation = ref([])

            //method register
            function register() {

                //define variable 
                let first_name = user.first_name
                let last_name = user.last_name
                let organization_name = user.organization_name
                let email = user.email
                let password = user.password
                let password_confirmation = user.password_confirmation

                //send server with axios
                axios.post('http://localhost:8000/api/register', {
                        first_name,
                        last_name,
                        organization_name,
                        email,
                        password,
                        password_confirmation
                })
                .then(() => {

                    //redirect ke halaman login
                    return router.push({
                        name: 'login'
                    })

                }).catch(error => {
                    //set validation dari error response
                    validation.value = error.response.data
                })

            }

            return {
                user, // <-- state user
                validation, // <-- state validation 
                register // <-- method register
            }

        }

    }
</script>