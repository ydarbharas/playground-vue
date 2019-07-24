<template>
    <div class="ma-3">
            <h2 class="headline">Number of Users:</h2>
            <span class="font-weight-bold display-2 orange--text">{{users.length}}</span>
        <v-container>
            <v-text-field
                class="mx-4"
                color="green"
                flat
                hide-details
                label="Search"
                prepend-inner-icon="search"
                solo-inverted
                v-model="searchString"
            ></v-text-field>
            <v-overflow-btn
                class="my-2"
                :items="companies"
                label="Company"
                target="#dropdown-example"
                v-model="selectedCompany"
            ></v-overflow-btn>
            <v-layout row wrap>
                <v-flex xs12 sm6 md4 v-for="(user, index) in filteredUsers" :key="index" class="pa-2">
                    <v-card :class="{'pink lighten-2' : isEven(index)}" justify-center class="text-xs-center pa-3 pink lighten-4">
                        <v-card-title class="d-block">
                            <p class="font-weight-bold headline ma-0">{{ user.name }}</p>
                        </v-card-title>
                        <div>
                            <v-avatar :size="80">
                                <v-img :src="user.image"></v-img>
                            </v-avatar>
                        </div>
                        <v-card-text>
                            <p><strong>Website: </strong> <a :href="user.website">{{user.website}}</a></p>
                            <p><strong>Company:</strong> {{ user.company }}</p>
                            <p><strong>Address:</strong> {{ user.city }}</p>
                        </v-card-text>
                    </v-card>
                </v-flex>
            </v-layout>
        </v-container>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            companies: ['All'],
            selectedCompany: '',
            users: [],
            pictures: [
                'https://images.unsplash.com/photo-1556228852-6d35a585d566?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=800&q=60', 
                'https://images.unsplash.com/photo-1556228453-622594a86e9b?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=668&q=80',
                'https://images.unsplash.com/photo-1556228852-6bf19d1b1fc5?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2533&q=80',
                'https://images.unsplash.com/photo-1438761681033-6461ffad8d80?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1500&q=80',
                'https://images.unsplash.com/photo-1501196354995-cbb51c65aaea?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1502&q=80',
                'https://images.unsplash.com/photo-1463453091185-61582044d556?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1500&q=80',
                'https://images.unsplash.com/photo-1500917293891-ef795e70e1f6?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1500&q=80',
                'https://images.unsplash.com/photo-1492446845049-9c50cc313f00?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=800&q=60',
                'https://images.unsplash.com/photo-1505503693641-1926193e8d57?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=800&q=60',
                'https://images.unsplash.com/photo-1506956191951-7a88da4435e5?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=800&q=60'],
            searchString: ''
        }
    },
    computed: {
        filteredUsers() {
            if (!this.searchString && !this.selectedCompany) {
                return this.users;
            } else if (this.searchString && !this.selectedCompany) {
                return this.users.filter((user) => {
                    if (user.name.toLowerCase().includes(this.searchString.toLowerCase())) {
                            return user;
                    }
                });
            } else if (!this.searchString && this.selectedCompany) {
                    return this.users.filter((user) => {
                        if (this.selectedCompany == 'All') {
                            return user
                        }
                        if (user.company == this.selectedCompany) {
                            return user;
                        }
                    });
            } else if (this.searchString && this.selectedCompany) {
                    return this.users.filter((user) => {
                        if (user.company == this.selectedCompany && 
                                user.name.toLowerCase().includes(this.searchString.toLowerCase()) && 
                                    this.selectedCompany !== 'All') {
                            return user;
                        } else if (user.name.toLowerCase().includes(this.searchString.toLowerCase()) && 
                                    this.selectedCompany == 'All') {
                            return user;
                        }
                    });
                }
                
        }
    },
    methods: {
        isEven(idx) {
            if ((idx % 2) == 0) {
                return true 
            } else {
                return false
            }
        },
        getAllUsers() {
            axios.get('https://jsonplaceholder.typicode.com/users')
                .then((response) => {
                    console.log(response);
                    let i = 0;
                    response.data.forEach(user => {
                        this.users.push({
                            website: user.website,
                            name: user.name,
                            company: user.company.name,
                            city: user.address.city,
                            image: this.pictures[i]
                        })
                        this.companies.push(user.company.name);
                        i++;
                    });
                })
                .catch((error) => {
                    console.log(error);
                })

        }
    },
    created() {
        this.getAllUsers()
    }
    
}
</script>

<style>

</style>
