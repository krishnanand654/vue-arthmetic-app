<template>
  <div class="login-ctn">
    <div>
      <label for="username">username </label><br />
      <input type="text" name="username" v-model.trim="username" required />
    </div>
    <div>
      <label for="password">password </label><br />
      <input type="password" v-model="password" @keyup.enter="performLogin" required />
    </div>
    <div class="login-btn-ctn">
      <button class="login-btn" @click="performLogin">Login</button>
    </div>
    <p>{{ error }}</p>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      username: '',
      password: '',
      token: null,
      error: null
    }
  },
  methods: {
    performLogin() {
      axios
        .post('http://localhost:3000/login', { username: this.username, password: this.password })
        .then((response) => {
          this.token = response.data.token
          this.error = null
          localStorage.setItem('token', this.token)
          this.$router.push('home')
        })
        .catch((error) => {
          this.error = error.response.data.message
        })
    }
  }
}
</script>

<style scoped>
input {
  background: rgb(30, 30, 30);
  border: 1px solid gray;
  height: 30px;
  border-radius: 5px;
  color: white;
  margin: 5px 0 10px;
  padding: 0 10px 0;
}

.login-ctn {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.login-btn-ctn {
  width: 180px;
}

.login-btn {
  margin: 20px 0 0;
  width: 100%;
  background: #29a569;
  padding: 5px;
  color: white;
  border: none;
  border-radius: 2px;
  font-weight: 600;
  cursor: pointer;
}
</style>
