<template>
  <div class="login-ctn">
    <div>
      <label for="username">Username </label><br />
      <input type="text" name="username" v-model.trim="username" required />
    </div>
    <div>
      <label for="password">Password </label><br />
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
.login-ctn label {
  font-size: 16px;
}
.login-ctn input {
  background: #2f2f2f;
  border: none;
  height: 35px;
  width: 250px;
  border-radius: 6px;
  color: white;
  margin: 5px 0 10px;
  padding: 0 10px 0;
}

.login-ctn input:focus {
  outline: 2px solid #29a569;
}

.login-ctn {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.login-btn-ctn {
  margin-bottom: 20px;
}

.login-btn {
  margin: 20px 0 0;
  width: 250px;
  background: #29a569;
  padding: 8px;
  font-size: 16px;
  color: white;
  border: none;
  border-radius: 5px;
  font-weight: 600;
  cursor: pointer;
}
</style>
