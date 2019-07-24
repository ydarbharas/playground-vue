<template>
    <div>
         <div>
    <v-form class="ma-4">

        <v-text-field
        v-model="newProject.projectTitle"
        label="Project title *"
        v-validate="'required'"
        data-vv-name="Project title"
        data-vv-as="project title"
        :error-messages="errors.collect('project title')">
        </v-text-field>

        <v-text-field
        v-model="newProject.clientName"
        label="Client name *"
        v-validate="'required'"
        data-vv-name="Client name"
        data-vv-as="client name"
        :error-messages="errors.collect('client name')">
        </v-text-field>

        <v-menu
        ref="startDateMenu"
        v-model="startDateMenu"
        :close-on-content-click="false"
        :return-value.sync="newProject.startDate"
        transition="scale-transition"
        offset-y
        full-width
        min-width="290px"
      >
        <template v-slot:activator="{ on }">
          <v-text-field
            v-model="formattedStartDate"
            label="Start date *"
            readonly
            v-on="on"
            v-validate="'required'"
            data-vv-name="project start date"
            :error-messages="errors.collect('project start date')"
          ></v-text-field>
        </template>
        <v-date-picker v-model="newProject.startDate" no-title scrollable>
          <v-spacer></v-spacer>
          <v-btn text color="grey" @click="startDateMenu = false">Cancel</v-btn>
          <v-btn text color="primary" @click="$refs.startDateMenu.save(newProject.startDate)">OK</v-btn>
        </v-date-picker>
      </v-menu>

       <v-menu
        ref="endDateMenu"
        v-model="endDateMenu"
        :close-on-content-click="false"
        :return-value.sync="newProject.endDate"
        transition="scale-transition"
        offset-y
        full-width
        min-width="290px"
      >
        <template v-slot:activator="{ on }">
          <v-text-field
            v-model="formattedEndDate"
            label="End date"
            readonly
            v-on="on"
          ></v-text-field>
        </template>
        <v-date-picker v-model="newProject.endDate" no-title scrollable>
          <v-spacer></v-spacer>
          <v-btn text color="grey" @click="endDateMenu = false">Cancel</v-btn>
          <v-btn text color="primary" @click="$refs.endDateMenu.save(newProject.endDate)">OK</v-btn>
        </v-date-picker>
      </v-menu> 
       
       <v-text-field
        v-model.number="newProject.rate"
        label="Hourly rate *"
        v-validate="'decimal:2'"
        data-vv-name="rate"
        prefix="£"
        :error-messages="errors.collect('rate')">
        </v-text-field>
        
        <v-text-field
        v-model="newProject.quantity"
        label="Hours worked *"
        v-validate="'numeric'"
        data-vv-name="quantity"
        :error-messages="errors.collect('quantity')">
        </v-text-field>

        <p class="headline">Total: £{{totalAmount}}</p>

            <v-btn @click="validateForm">Create Project</v-btn>
    </v-form>
    </div>
    <!-- <div class="ma-4 headline">
        <p>Project created</p>
    </div> -->
    </div>
</template>

<script>
export default {
    data() {
        return {
            newProject: {
                projectTitle: '',
                clientName: '',
                startDate: undefined,
                endDate: undefined,
                rate: undefined,
                quantity: undefined
            },
            startDateMenu: false,
            endDateMenu: false,
        }
    },
    computed: {
        formattedStartDate() {
            if (this.newProject.startDate) {
                return this.formatDate(this.newProject.startDate);
            }
        },
        formattedEndDate() {
            if (this.newProject.endDate) {
                return this.formatDate(this.newProject.endDate);
            }
        },
        totalAmount() {
            if (this.newProject.rate && this.newProject.quantity) {
                return this.newProject.rate * this.newProject.quantity;
            }
        }
    },
    methods: {
        formatDate(date) {
            var newDate = new Date(date);
            return `${newDate.getDate()}/${newDate.getMonth() + 1}/${newDate.getFullYear()}`;
        },
        validateForm() {
            this.$validator.validateAll()
            .then((result) => {
                if (result) {
                    this.saveData();
                } else {
                    // validation failed.
                }
            })
            .catch( (error) =>  {
                // something serious has went wrong, not a validation error.
            });
        },
        saveData() {
            let formData = new FormData();
            formData.append('Project title:', this.newProject.projectTitle);
            formData.append('Client name:', this.newProject.clientName);
            formData.append('Start date:', this.newProject.startDate);
            if (this.newProject.endDate) {
                formData.append('End date:', this.newProject.endDate);
            }
            formData.append('Rate:', this.newProject.rate);
            formData.append('Quantity:', this.newProject.quantity);
            formData.append('Total:', this.newProject.rate * this.newProject.quantity);
            for(var pair of formData.entries()) {
                console.log(pair[0]+ ', '+ pair[1]); 
                }
            }
        }
    }
</script>

<style>

</style>
