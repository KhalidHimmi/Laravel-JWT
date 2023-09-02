<template>
    <div>

        <!-- Modal trigger button -->
        <button type="button" name="" id="" class="btn btn-primary" data-bs-toggle="modal"
            data-bs-target="#modalId">Login</button>
        <button class="btn btn-secondary" @click="logout">Log out</button>

        <button @click="display">Display</button>

        <!-- Modal Body -->
        <!-- if you want to close by clicking outside the modal, delete the last endpoint:data-bs-backdrop and data-bs-keyboard -->
        <div class="modal fade" id="modalId" tabindex="-1" data-bs-backdrop="static" data-bs-keyboard="false"
            role="dialog" aria-labelledby="modalTitleId" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered modal-sm" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="modalTitleId">Login</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <div >
                            <p class="alert alert-success" v-if="message != ''">{{message}}</p>
                        </div>
                        <div class="mb-3">
                          <label for="" class="form-label">Email</label>
                          <input type="email" name="email" v-model="data.email" class="form-control" placeholder="Email">
                        </div>
                        <div class="mb-3">
                          <label for="" class="form-label">Password</label>
                          <input type="password" name="password" v-model="data.password" class="form-control" placeholder="Password">
                        </div>
                        <div class="mb-3">
                            <button class="btn btn-primary" @click="login">Login</button>
                        </div>
                    </div>                  
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import axios from 'axios';

export default {
    data() {
        return {
            data: {email: "",password: ""},
            logged: localStorage.getItem('token') ? true : false,
            // token: localStorage.getItem('token') || "",
            token: localStorage.getItem('token') ? JSON.parse(localStorage.getItem('token')) : '',
            id: "",
            message: "",
        }
    },
    methods: {
        login(){
            axios.post('/api/login',this.getData)
                .then((res) => {
                    //  console.log(res.data.authorisation.token);
                     this.token = res.data.authorisation.token;
                    //  this.id = res.data.user.id;
                    //  this.message = res.data.status;
                    //  this.displayID();
                     localStorage.setItem('token',JSON.stringify(this.token))
                })
                .catch(err => {
                    console.log(err);
                    this.message = "";
                })
        },
        logout(){
            localStorage.removeItem('token');
            this.token = '';
        },
        displayID(){
            console.log("The id is " + this.id);
        },
        display(){
            console.log(this.logged);
            console.log(this.token);
        }
    },
    computed:{
        getData(){
            return this.data;
        },
    }
}
</script>
<style>

</style>