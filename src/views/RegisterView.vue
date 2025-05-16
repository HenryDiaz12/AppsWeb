<template>
  <div class="auth">
    <h1>Registro</h1>
    <form @submit.prevent="register">
      <div>
        <label>Nombre:</label>
        <input v-model="user.name" type="text" required>
      </div>
      
      <div>
        <label>Email:</label>
        <input v-model="user.email" type="email" required>
      </div>
      
      <div>
        <label>Contraseña:</label>
        <input v-model="user.password" type="password" required>
      </div>
      
      <button type="submit">Registrarse</button>
    </form>
    
    <p>¿Ya tienes cuenta? <router-link to="/login">Inicia sesión</router-link></p>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const user = ref({
  name: '',
  email: '',
  password: ''
})

const register = async () => {
  try {
    const users = JSON.parse(localStorage.getItem('users')) || []
    users.push(user.value)
    localStorage.setItem('users', JSON.stringify(users))
    

    router.push('/login')
  } catch (error) {
    console.error('Error:', error)
  }
}
</script>

<style>
.auth {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
}

form div {
  margin-bottom: 15px;
}

label {
  display: block;
  margin-bottom: 5px;
}

input {
  width: 100%;
  padding: 8px;
  box-sizing: border-box;
}

button {
  width: 100%;
  padding: 10px;
  background: #2e7d32;
  color: white;
  border: none;
  cursor: pointer;
}

button:hover {
  background: #1b5e20;
}
</style>