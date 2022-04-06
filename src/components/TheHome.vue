<script setup lang="ts">
import {ref,getCurrentInstance} from "vue";
import axios from "axios";
import $ from "jquery";
import router from "@/router";

const txtemail = ref('')
const txtpass = ref('')
const loader = ref(false)
const isDisabled = ref(false)
const isHidden = ref(false)
const isAlert = ref(true)
const message = ref('')
const isClass = ref('')
const appid = getCurrentInstance()?.appContext.config.globalProperties.$applicationId;


$('.modal-backdrop').remove();

function handleInput(email: any,pass: any){
        axios.post('http://localhost/beapp/api/v1/auth',
        JSON.stringify({
            appid: appid,
            email: email,
            password: pass,
        }), 
        {
            headers:{
                'Content-type': 'application/json; charset=UTF-8'
            }

        })
        .then(response => {
            var res = response.data;
            var code = res.code;
            var resmessage = res.message;
            var userData = res.data.user;
            var token = res.data.type+" "+res.data.token;
            //console.log(res);

            isDisabled.value = false
            isHidden.value = false
            loader.value = false

            if(code == '01'){
                isAlert.value = false
                isClass.value = 'alert alert-warning'
                message.value = resmessage
            }else if(code == '00'){
                isAlert.value = false
                isClass.value = 'alert alert-success'
                message.value = resmessage

                //save to client storage
                localStorage.setItem("id",userData.id);
                localStorage.setItem("user_id",userData.user_id);
                localStorage.setItem("fullname",userData.fullname);
                localStorage.setItem("email",userData.email);
                localStorage.setItem("token",token);

                router.push('/main')

                //console.log(res.data.token);
            }
        })
        .catch(error => {
            isDisabled.value = false
            isHidden.value = false
            loader.value = false
            isAlert.value = false
            isClass.value = 'alert alert-danger'
            message.value = 'Ooppss.. disconected! Please try again'
        })

}
</script>

<template>
    <body class="bg-gradient-primary">

        <div class="container">

            <!-- Outer Row -->
            <div class="row justify-content-center">

                <div class="col-xl-10 col-lg-12 col-md-9">

                    <div class="card o-hidden border-0 shadow-lg my-5">
                        <div class="card-body p-0">
                            <!-- Nested Row within Card Body -->
                            <div class="row">
                                <div class="col-lg-6 d-none d-lg-block bg-login-image"></div>
                                <div class="col-lg-6">
                                    <div class="p-5">
                                        <div class="text-center">&nbsp;</div>
                                        <div class="text-center">&nbsp;</div>
                                        <div class="text-center">&nbsp;</div>
                                        <div class="text-center">
                                            <h1 class="h4 text-gray-900 mb-4">Welcome Back!</h1>
                                        </div>
                                        <form class="user">
                                            <div class="form-group">
                                                <div :class="isClass" role="alert" :hidden="isAlert">
                                                {{message}}
                                                </div>
                                            </div>
                                            <div class="form-group">
                                                <input type="email" v-model="txtemail" class="form-control form-control-user"
                                                    id="exampleInputEmail" aria-describedby="emailHelp"
                                                    placeholder="Enter Email Address...">
                                            </div>
                                            <div class="form-group">
                                                <input type="password" v-model="txtpass" class="form-control form-control-user"
                                                    id="exampleInputPassword" placeholder="Password">
                                            </div>
                                            <div class="form-group">
                                                <div class="custom-control custom-checkbox small">
                                                    <input type="checkbox" class="custom-control-input" id="customCheck">
                                                    <label class="custom-control-label" for="customCheck">Remember
                                                        Me</label>
                                                </div>
                                            </div>
                                            <button type="submit" :hidden="isHidden" class="btn btn-primary btn-user btn-block" :disabled='isDisabled' @click.prevent="handleInput(txtemail,txtpass);loader = !loader;isDisabled = true;isHidden = true;isAlert=true;">
                                            Login
                                            </button>
                                            <button class="btn btn-primary btn-user btn-block" type="button" disabled v-if="loader">
                                                <span class="spinner-grow spinner-grow-sm" role="status" aria-hidden="true"></span>
                                                Loading...
                                            </button>
                                        </form>
                                        <div class="text-center">&nbsp;</div>
                                        <div class="text-center">&nbsp;</div>
                                        <div class="text-center">&nbsp;</div>
                                        <div class="text-center">&nbsp;</div>
                                        <div class="text-center">&nbsp;</div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                </div>

            </div>

        </div>
    </body>
</template>