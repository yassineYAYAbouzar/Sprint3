<template>
  <v-container class="py-8 px-6" fluid>
    <v-row>
      <v-col cols="5" class="mx-auto mt-15 form-custem">
        <v-alert type="error" v-model="error">
          Email Or Password Incorrect !
        </v-alert>
        <h1 class="text-center">Reset Password</h1>
        
        <v-form ref="form" v-model="valid" lazy-validation>
          <v-text-field
            v-model="form.Password"
            :counter="6"
            :rules="passwordRules"
            label="Password"
            required
          ></v-text-field>
          <v-text-field
            v-model="form.Password"
            :counter="6"
            :rules="passwordRules"
            label="Password"
            required
          ></v-text-field>
          <v-btn
            :disabled="!valid"
            color="success"
            class="mr-4"
            @click="Submits"
          >
            Validate
          </v-btn>
          <v-btn color="error" class="mr-4" @click="reset"> Reset Form </v-btn>
        </v-form>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
import axios from 'axios';
import Swal from 'sweetalert2';
export default {
  name: "resetPassword",
    data: () => ({
      valid: true,
      error:false,
      form:{
        Email: '',
        Password: '',
      },
      
      nameRules: [
        v => !!v || '',
        v => (v && v.length <= 0) || 'Name must be less than 10 characters',
      ],
      
      passwordRules: [
        v => !!v || '',
        v => (v && v.length >= 0) || 'Password must be less than 10 characters',
      ],
      emailRules: [
        v => !!v || 'E-mail is required',
        v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
      ],
    }),

    methods: {
      validate () {
        this.$refs.form.validate()
      },
      reset () {
        this.$refs.form.reset()
      },
    
       Submits () {
        this.form.Email = this.$route.query.email
        var data = new FormData();
        data.append("Password", this.form.Password);
        data.append("Email", this.form.Email);
        axios.post('http://localhost/Sprint3_brief/backend/views/updatePassword.php', data)
          .then(response => {
            
            if (response.data =="success") {
              Swal.fire({
                type: 'succsess',
                title: 'Password  !',
                text: 'Password Modifié Avec Succés!',
              })
              this.$router.push("/login")
            }
            
          })
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

      