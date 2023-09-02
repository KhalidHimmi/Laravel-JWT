<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card mt-5">
                    <div class="card-header">
                        Contacts
                        <a class="btn btn-sm btn-primary" data-bs-toggle="modal" href="#exampleModalToggle"
                            role="button" id="addContact">Add</a>
                    </div>
                    <div class="card-body" v-for="contact in contacts" :key="contact.id">
                        <ul class="list-group">
                            <li class="list-group-item d-flex">
                                <!-- <div> -->
                                <span>{{ contact.name }}</span>
                                <span style="color: blue">{{
                                contact.phone
                                }}</span>
                                <!-- </div> -->
                                <div class="ud">
                                    <span>
                                        <button class="btn btn-sm btn-warning" @click="getID(contact.id)" 
                                        data-bs-toggle="modal" href="#exampleModalToggle">Update</button>
                                    </span>
                                    <span>
                                        <button class="btn btn-sm btn-danger" @click="deleteContact(contact.id)">Delete</button>
                                    </span>
                                </div>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
        <div class="modal fade" id="exampleModalToggle" aria-hidden="true" aria-labelledby="exampleModalToggleLabel"
            tabindex="-1">
            <div class="modal-dialog modal-dialog-centered">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalToggleLabel">
                            Add contact
                        </h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <form>
                            <div class="mb-3">
                                <label for="exampleInputEmail1" class="form-label">Name : </label>
                                <input type="text" class="form-control" name="name" v-model="contact.name" placeholder="Contact name" />
                            </div>
                            <div class="mb-3">
                                <label for="phone" class="form-label">Phone : </label>
                                <input type="text" class="form-control" name="phone" v-model="contact.phone" placeholder="Contact phone" />
                            </div>                     
                            <button v-if="!updating" type="button" class="btn btn-primary" @click="addContact">
                                Submit
                            </button>
                            <button v-else type="button" class="btn btn-warning" @click="updateContact(contact.id)">
                                Update
                            </button>
                        </form>
                    </div>              
                </div>
            </div>
        </div>
        <Login/>
    </div>
</template>

<script>
import axios from "axios";
// import Login from 'Login.vue';
import Login from "./Login.vue";

export default {
    data() {
        return {
            contacts: [],
            contact: { name: "", phone: "" },
            updating: false,
        };
    },
    component: {
        Login,
    },
    methods: {
        getContact() {
            axios
                .get("/api/contacts")
                .then((response) => {
                // console.log(response.data.data);
                this.contacts = response.data.data;
            })
                .catch((err) => console.log(err));
        },
        addContact() {
            axios.post("/api/contacts", this.contact)
                .then(response => {
                this.getContact();
                this.contact = { name: "", phone: "" };
            })
                .catch(error => console.log(error));
        },
        updateContact(id) {
            axios.put("/api/contacts/" + id, this.contact)
                .then(response => {
                this.getContact();
                this.contact = { name: "", phone: "" };
            })
                .catch(error => console.log(error));
        },
        getID(id) {
            this.updating = true;
            axios.get("/api/contacts/" + id)
                .then(response => {
                this.contact = response.data.data;
                console.log(this.contact);
            })
                .catch(error => console.log(error));
        },
        deleteContact(id) {
            Swal.fire({
                title: "Are you sure?",
                text: "You won't be able to revert this!",
                icon: "warning",
                showCancelButton: true,
                confirmButtonColor: "#3085d6",
                cancelButtonColor: "#d33",
                confirmButtonText: "Yes, delete it!"
            }).then((result) => {
                if (result.isConfirmed) {
                    axios.delete("/api/contacts/" + id)
                        .then(response => {
                        this.getContact();
                    })
                        .catch(error => console.log(error));
                    Swal.fire("Deleted!", "Your file has been deleted.", "success");
                }
            });
        }
    },
    mounted() {
        this.getContact();
    },
    components: { Login }
};
</script>

<style>
li {
    align-items: center;
    justify-content: space-between;
}

.btn-danger {
    margin-left: 1.5rem;
}

.card-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
}
</style>
