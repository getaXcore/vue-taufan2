<script setup lang="ts">
import {ref,getCurrentInstance} from "vue";
import router from "@/router";
import axios from "axios";
import Sidebar from "../components/Sidebar.vue";
import Topbar from "../components/Topbar.vue";
import Footer from "../components/Footer.vue";
import $ from "jquery";

let email = localStorage.getItem('email');
let token = localStorage.getItem('token');
const appid = getCurrentInstance()?.appContext.config.globalProperties.$applicationId;

if(email != null && token != null){
     axios.post('http://localhost/beapp/api/v1/sessioncheck',
        JSON.stringify({
            appid: appid,
            email: email
        }), 
        {
            headers:{
                'Content-type': 'application/json; charset=UTF-8',
                'Authorization' : token
            }
        })
        .then(response => {
            var res = response.data;
            var code = res.code;

            if(code != "00"){ // session expired
               router.push('/'); //return to login
            }            
        })
        .catch(error => {
            console.log(error);
            router.push('/'); //return to login
        })
}else{
    router.push('/');//return to login
}

function logout(){
    axios.post('http://localhost/beapp/api/v1/logout',
        JSON.stringify({
            appid: appid,
            email: email
        }), 
        {
            headers:{
                'Content-type': 'application/json; charset=UTF-8',
                'Authorization' : token
            }
        })
        .then(response => {
            var res = response.data;
            var code = res.code;

            if(code == "00"){ // logout success
               router.push('/'); //return to login
               
               //console.log(response);
            }else{
                //alert error
            }
        })
        .catch(error => {
            console.log(error);
            //router.push('/'); //return to login
            //alert error
        })
}

</script>
<template>
    <body id="page-top">
        <!-- Page Wrapper -->
        <div id="wrapper">
            <Sidebar />
            <!-- Content Wrapper -->
            <div id="content-wrapper" class="d-flex flex-column">

                <!-- Main Content -->
                <div id="content">
                    <Topbar />
                    <!-- Begin Page Content -->
                    <div class="container-fluid">
                    </div>
                </div>
                <!-- End of Main Content -->
                <Footer />
            </div>
            <!-- End of Content Wrapper -->
        </div>
        <!-- End of Page Wrapper -->

        <!-- Scroll to Top Button-->
        <a class="scroll-to-top rounded" href="#page-top">
            <i class="fas fa-angle-up"></i>
        </a>

        <!-- Logout Modal-->
        <div class="modal fade" id="logoutModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel"
            aria-hidden="true">
            <div id="logoutModal" class="modal-dialog" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalLabel">Ready to Leave?</h5>
                        <button class="close" type="button" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">×</span>
                        </button>
                    </div>
                    <div class="modal-body">Select "Logout" below if you are ready to end your current session.</div>
                    <div class="modal-footer">
                        <button class="btn btn-secondary" type="button" data-dismiss="modal">Cancel</button>
                        <button class="btn btn-primary" @click="logout()">Logout</button>
                    </div>
                </div>
            </div>
        </div>
    </body>
</template>