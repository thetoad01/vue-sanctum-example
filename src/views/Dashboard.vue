<template>
  <div class="dashboard">
    <h1>Dashboard view</h1>

    <div v-if="auth">
      <div>Email: {{ email }}</div>
      <button @click="logout">Logout</button>
    </div>

    <h1 v-else>Please log in!</h1>
  </div>
</template>

<script>
import axios from 'axios';

axios.defaults.withCredentials = true;
axios.defaults.baseURL = 'http://localhost:8000';

// Add a request interceptor
axios.interceptors.request.use((config) => {
    // Do something before request is sent
    return config;
  }, (error) => {
    // Do something with request error
    return Promise.reject(error);
  });
// Add a response interceptor
axios.interceptors.response.use((response) => {
    // Do something with response data
    return response;
  }, (error) => {
    // Do something with response error
    return Promise.reject(error);
  });

export default {
  data() {
    return {
      auth: false,
      email: '',
    }
  },
  mounted() {
    axios.get('/api/user')
    .then(response => {
      this.auth = true;
      this.email = response.data.email;
    })
    .catch(error => {
      if (error.response) {
        this.auth = false;
        // console.log(error.response.data);
        // console.log(error.response.status);
        // console.log(error.response.headers);
      } else {
        console.log(error);
      }
    });
  },
  methods: {
    logout() {
      axios.post('/logout').then(response => {
        this.$router.push({ name: 'Home' });
      });
    }
  }
}
</script>