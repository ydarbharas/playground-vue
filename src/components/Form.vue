<template>
<div>
    <div v-if="editing">
    <v-form class="ma-4">

        <v-text-field
        v-model="postData.title"
        v-validate="'required|min:3'"
        data-vv-name="title"
        data-vv-as="title"
        :error-messages="errors.collect('title')"
        label="Title">
        </v-text-field>

        <v-text-field
        v-model="postData.body"
        label="Body"
        v-validate="'required|min:10'"
        data-vv-name="body"
        data-vv-as="body"
        :error-messages="errors.collect('title')">
        </v-text-field>
            <v-btn @click="validateForm">submit</v-btn>
            <v-btn @click="clearForm">clear</v-btn>
            <v-btn @click="editForm">edit</v-btn>
    </v-form>
    </div>
    <div class="ma-4 headline" v-else>
        <p>Post edited!</p>
    </div>
</div>
</template>

<script>
import axios from 'axios'

export default {
    data() {
        return {
            postData: {
                title: '',
                body: '',
                userId: 1
            },
            editing: true
        }
    },
    methods: {
        validateForm() {
            this.$validator.validateAll()
            .then((result) => {
                if (result) {
                    this.savePost();
                } else {
                    // validation failed.
                }
            })
            .catch( (error) =>  {
                // something serious has went wrong, not a validation error.
            });
        },
        savePost() {
            axios.post('https://jsonplaceholder.typicode.com/posts', JSON.stringify(this.postData))
                .then((response) => {
                    console.log(response);
                })
                .catch((error) => {
                    console.log(error);
                })
        },
        editForm() {
            axios.put('https://jsonplaceholder.typicode.com/posts/51', JSON.stringify(this.postData))
                .then((response) => {
                    console.log((response))
                    this.editing = false;

                })
                .catch((error) => {
                    console.log(error);
                })
            
        },
        clearForm() {
            this.postData.title = '';
            this.postData.body = '';
        },
        fiftyOne() {
            axios.get('https://jsonplaceholder.typicode.com/posts/51')
                .then((response) => {
                this.postData.title = response.data.title;
                this.postData.body = response.data.body;
                })
                .catch((error) => {
                        console.log(error);
                    })
        }
    },
    // mounted() {
    //     this.fiftyOne()
    // }

}
</script>

<style>

</style>
