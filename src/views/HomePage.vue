<template>
    <div>
        <div class="container">
            <h1>Cloudpunch Candy</h1>
            <h4>We have many candy options! Add your own to the list, edit, or delete an existing candy.</h4>
        </div>

        <!-- Div for GET candy -->
        <div v-for="candy in candy"
        :key="candy.candyId"
        class="container"
        >
            <p>Id: {{ candy.candyId }} {{ candy.candyName }} | {{ candy.category }}</p>
        </div>

        <!-- Div for POST candy -->
        <div>
            <v-container>
                <h3>Add to Candy:</h3>
                <v-col>
                    <v-text-field
                    v-model="candyName"
                    label="Candy Name"
                    >
                    </v-text-field>
                </v-col>
                <v-col>
                    <v-text-field
                    v-model="category"
                    label="Category"
                    >
                    </v-text-field>
                    <button @click="addCandy">Submit</button>
                    <div v-if="message">
                        <p>{{ message }}</p>
                    </div>
                </v-col>
            </v-container>
        </div>

        <!-- Div for PATCH candy -->
        <div>
            <v-container>
                <h3>Edit Existing Candy:</h3>
                <v-col>
                    <v-text-field
                    v-model="candyId"
                    label="Candy Id"
                    >
                    </v-text-field>
                    <v-text-field
                    v-model="candyName"
                    label="Candy Name"
                    >
                    </v-text-field>
                    <button @click="editCandy">Submit</button>
                    <div v-if="message">
                        <p>{{ message }}</p>
                    </div>
                </v-col>
            </v-container>
        </div>

        <!-- Div for DELETE candy -->
        <div>
            <v-container>
                <h3>Delete Candy:</h3>
                <v-col>
                    <v-text-field
                    v-model="candyName"
                    label="Candy Name"
                    >
                    </v-text-field>
                    <button @click="deleteCandy">Submit</button>
                    <div v-if="message">
                        <p>{{ message }}</p>
                    </div>
                </v-col>
            </v-container>
        </div>

    </div>
</template>

<script>
import axios from "axios";

    export default {
        name: "HomePage",
        data() {
            return {
                candy: [],
                candyName: "",
                candyId: "",
                category: "",
                message: ""
            }
        },
        methods: {
            getCandy() {
                axios.request({
                    url: `${process.env.VUE_APP_BASE_DOMAIN}/api/candy`,
                    method: "GET",
                }).then((response)=>{
                    this.candy = response.data;
                }).catch((error)=>{
                    error = "Something went wrong, try again."
                    alert(error);
                })
            },
            addCandy() {
                axios.request({
                    url: `${process.env.VUE_APP_BASE_DOMAIN}/api/candy`,
                    method: "POST",
                    data: {
                        candyName: this.candyName,
                        category: this.category
                    }
                }).then(()=>{
                    this.clearTextBox();
                    this.message = "Successfully added candy!";
                    
                }).catch((error)=>{
                    this.clearTextBox();
                    this.message = error;
                })
            },
            editCandy() {
                axios.request({
                    url: `${process.env.VUE_APP_BASE_DOMAIN}/api/candy`,
                    method: "PATCH",
                    data: {
                        candyId: this.candyId,
                        candyName: this.candyName,
                    }
                }).then(()=>{
                    this.clearTextBox();
                    this.message = "Successfully edited candy!";
                    
                }).catch((error)=>{
                    this.clearTextBox();
                    this.message = error;
                })
            },
            deleteCandy() {
                axios.request({
                    url: `${process.env.VUE_APP_BASE_DOMAIN}/api/candy`,
                    method: "DELETE",
                    data: {
                        candyName: this.candyName,
                    }
                }).then(()=>{
                    this.clearTextBox();
                    this.message = "Successfully deleted candy!";
                    
                }).catch((error)=>{
                    this.clearTextBox();
                    this.message = error;
                })
            },
            clearTextBox(){
                this.candyName = "";
                this.category = "";
                this.candyId = "";
            }
        },
        mounted () {
            this.getCandy();
        },
    }
</script>

<style scoped>
.container{
    width: 75vw;
}
button{
    background-color: lightblue;
    padding: 5px;
}
</style>