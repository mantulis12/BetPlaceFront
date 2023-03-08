<template>
  <div class="col-md-12">
    <label>{{ errorMessage }}</label>
    <input class="input-group input-lg" v-model="username" placeholder="username"/>
    <input class="input-group input-lg" v-model="password" type="password" placeholder="passowrd"/>
    <input @click="login()" class="btn btn-default" type="submit"/>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      errorMessage: "Error"
    }
  },
  methods: {
    login() {
      axios.post('https://localhost:7200/api/login', {
        username: this.username,
        password: this.password
      })
          .then(response => {
            // Save token to local storage
            localStorage.setItem('token', response.data.token);

            // Redirect to dashboard page
            this.$router.push('/about');
          })
          .catch(error => {
            // Display error message
            this.errorMessage = error.response.data.message;
          });
    }
  }
}
</script>
