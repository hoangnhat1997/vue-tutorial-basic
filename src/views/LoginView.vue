<template>
  <div class="login-container">
    <h2>Login</h2>
    <form @submit.prevent="handleSubmit">
      <div class="form-group">
        <label for="username">Username</label>
        <input
          type="text"
          v-model="username"
          id="username"
          placeholder="Enter your username"
          required
        />
      </div>

      <div class="form-group">
        <label for="password">Password</label>
        <input
          type="password"
          v-model="password"
          id="password"
          placeholder="Enter your password"
          required
        />
      </div>

      <button type="submit" :disabled="loading">
        {{ loading ? 'Logging in...' : 'Login' }}
      </button>
    </form>

    <p v-if="error" class="error-message">{{ error }}</p>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      username: '',
      password: '',
      loading: false,
      error: null
    }
  },
  methods: {
    async handleSubmit() {
      if (this.username && this.password) {
        this.loading = true
        this.error = null

        try {
          const response = await axios.post('https://example.com/api/login', {
            username: this.username,
            password: this.password
          })

          // Handle success, for example:
          console.log('Login successful:', response.data)
          alert('Login successful!') // Show success alert
        } catch (error) {
          // Handle error
          console.error('Login failed:', error.response.data)
          this.error = error.response?.data?.message || 'Login failed. Please try again.'
        } finally {
          this.loading = false
        }
      } else {
        this.error = 'Please enter your username and password'
      }
    }
  }
}
</script>

<style scoped>
.login-container {
  max-width: 300px;
  margin: auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 4px;
  background-color: #f9f9f9;
}

h2 {
  text-align: center;
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 15px;
}

label {
  display: block;
  margin-bottom: 5px;
}

input[type='text'],
input[type='password'] {
  width: 100%;
  padding: 8px;
  box-sizing: border-box;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  width: 100%;
  padding: 10px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:disabled {
  background-color: #0056b3;
  cursor: not-allowed;
}

.error-message {
  color: red;
  margin-top: 15px;
  text-align: center;
}
</style>
