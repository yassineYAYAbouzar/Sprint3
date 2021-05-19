<template>
<div>
    
      <app-bar class="mx-auto my-16"/>
       <v-container>
 <v-card
    class="mx-auto my-16"
    max-width=" 544"
    outlined
    
  >
     <v-card
    class="overflow-hidden"
    color="light-blue accent-3"
    dark
  >
    <v-toolbar
      flat
      color="primary"
    >
      <v-icon>mdi-account</v-icon>
      <v-toolbar-title class="font-weight-light">
        User Profile
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        color="light-blue accent-3"
        fab
        small
        @click="isEditing = !isEditing"
      >
        <v-icon v-if="isEditing">
          mdi-close
        </v-icon>
        <v-icon v-else>
          mdi-pencil
        </v-icon>
      </v-btn>
    </v-toolbar>
    <v-card-text>
     
          <v-text-field
          :disabled="!isEditing"
            v-model="UserProfile.FirtName"
            :counter="5"
            :rules="nameRules"
            label="First Name"
            required
          ></v-text-field>
          <v-text-field
          :disabled="!isEditing"
            v-model="UserProfile.LastName"
            :counter="5"
            :rules="nameRules"
            label="Last Name"
            required
          ></v-text-field>
          <v-text-field
            :disabled="!isEditing"
            v-model="UserProfile.Email"
            :rules="emailRules"
            label="E-mail"
            required
          ></v-text-field>
          <v-text-field
            :disabled="!isEditing"
            v-model="UserProfile.Password"
            :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
            :type="show1 ? 'text' : 'password'"
            name="input-10-1"
            label="Normal with hint text"
            hint="At least 8 characters"
            counter
            required
            @click:append="show1 = show1"
          ></v-text-field>
          <v-text-field
            v-model="UserProfile.Password"
            class="d-none"
          ></v-text-field>
          <v-text-field
            v-model="UserProfile.id"
            class="d-none"
          ></v-text-field>
    </v-card-text>
    <v-divider></v-divider>
    <v-card-actions>
      <v-spacer></v-spacer>
      <v-btn
        :disabled="!isEditing"
        color="success"
        @click="click"
      >
        Save
      </v-btn>
    </v-card-actions>
    <v-snackbar
      v-model="hasSaved"
      :timeout="2000"
      absolute
      bottom
      left
    >
      Your profile has been updated
    </v-snackbar>
  </v-card>
  </v-card>
   </v-container>
    <Footer-app />
  </div>
</template>
<script>
import axios from 'axios';
import Swal from 'sweetalert2';

import appBar from '../components/appBar.vue';
import FooterApp from '../components/FooterApp.vue';

export default {
    components: { appBar,FooterApp},
  name: "Profile",
    data: () => ({
      show1: false,
      valid: true,
      hasSaved: false,
        isEditing: null,
        model: null,
      UserProfile:{
        FirtName: '',
        LastName: '',
        Password: '',
        Email: '',
        ID: '',
      },
      nameRules: [
        v => !!v || '',
        v => (v && v.length <= 5) || 'Name must be less than 10 characters',
      ],
      
      passwordRules: [
        v => !!v || '',
        v => (v && v.length <= 6) || 'Password must be less than 10 characters',
      ],
      emailRules: [
        v => !!v || 'E-mail is required',
        v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
      ],
    }),
    mounted(){
      var Role = JSON.parse(sessionStorage.getItem("userAuth"));
          this.UserProfile =Role;
    },
    methods: {
      validate () {
        this.$refs.form.validate()
      },
      click () {
        var data = new FormData();
        data.append("FirtName", this.UserProfile.FirtName);
        data.append("id", this.UserProfile.id);
        data.append("LastName", this.UserProfile.LastName);
        data.append("Email", this.UserProfile.Email);
        data.append("Password", this.UserProfile.Password);
        axios
          .post('http://localhost/Sprint3_brief/backend/views/updateUesr.php',data)
          .then(()=>{
            Swal.fire({
              type: 'succsess',
              title: 'Updated !',
              text: 'Info Updated Avec Succés You need To signOut!',
          })
            this.$router.push("/login")
          })
      },
        reset () {
        this.$refs.form.reset()
      },
    },
};
</script>

<style>
.form-custem{
    background: #f7faff;
    padding: 31px;
    border-radius: 21px;
    box-shadow: 1px 10px 21px -7px;
}
</style>

