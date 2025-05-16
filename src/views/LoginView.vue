<template>
  <section class="auth-container">
    <div class="auth-card">
      <h1 class="auth-title">Iniciar Sesión</h1>
      
      <form @submit.prevent="handleLogin" class="auth-form" novalidate>
        <div class="form-group">
          <label for="email" class="form-label">Correo electrónico</label>
          <input
            type="email"
            id="email"
            v-model.trim="email"
            required
            autocomplete="username"
            class="form-input"
            :class="{ 'input-error': formErrors.email }"
          />
          <span v-if="formErrors.email" class="error-text">{{ formErrors.email }}</span>
        </div>

        <div class="form-group">
          <label for="password" class="form-label">Contraseña</label>
          <input
            type="password"
            id="password"
            v-model.trim="password"
            required
            autocomplete="current-password"
            class="form-input"
            :class="{ 'input-error': formErrors.password }"
          />
          <span v-if="formErrors.password" class="error-text">{{ formErrors.password }}</span>
        </div>

        <div v-if="authError" class="auth-error">
          {{ authError }}
        </div>

        <button type="submit" class="auth-button" :disabled="isSubmitting">
          <span v-if="!isSubmitting">Ingresar</span>
          <span v-else class="loading-indicator">Procesando...</span>
        </button>
      </form>

      <div class="auth-footer">
        ¿No tienes cuenta? 
        <router-link to="/register" class="auth-link">Regístrate aquí</router-link>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'


const router = useRouter()


const email = ref('')
const password = ref('')


const isSubmitting = ref(false)
const authError = ref('')
const formErrors = ref({
  email: '',
  password: ''
})


const validateForm = () => {
  let isValid = true
  formErrors.value = { email: '', password: '' }

  if (!email.value) {
    formErrors.value.email = 'El correo es obligatorio'
    isValid = false
  } else if (!/^\S+@\S+\.\S+$/.test(email.value)) {
    formErrors.value.email = 'Ingresa un correo válido'
    isValid = false
  }

  if (!password.value) {
    formErrors.value.password = 'La contraseña es obligatoria'
    isValid = false
  } else if (password.value.length < 6) {
    formErrors.value.password = 'Mínimo 6 caracteres'
    isValid = false
  }

  return isValid
}

const handleLogin = async () => {
  if (!validateForm()) return

  isSubmitting.value = true
  authError.value = ''

  try {
    const users = JSON.parse(localStorage.getItem('users')) || []
    const user = users.find(u => u.email === email.value)

    if (!user) {
      authError.value = 'Credenciales incorrectas'
      return
    }

 
    if (user.password !== password.value) {
      authError.value = 'Credenciales incorrectas'
      return
    }

  
    localStorage.setItem('currentUser', JSON.stringify({
      email: user.email,
      name: user.name,

    }))


    router.push('/dashboard')
  } catch (error) {
    console.error('Login error:', error)
    authError.value = 'Ocurrió un error. Intenta nuevamente.'
  } finally {
    isSubmitting.value = false
  }
}
</script>

<style scoped>
.auth-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #f5f5f5;
  padding: 1rem;
}

.auth-card {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  padding: 2rem;
  width: 100%;
  max-width: 400px;
}

.auth-title {
  color: #2E7D32;
  text-align: center;
  margin-bottom: 1.5rem;
  font-size: 1.5rem;
}

.auth-form {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.form-label {
  font-weight: 500;
  color: #333;
}

.form-input {
  padding: 0.75rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 1rem;
}

.form-input:focus {
  outline: none;
  border-color: #2E7D32;
  box-shadow: 0 0 0 2px rgba(46, 125, 50, 0.2);
}

.input-error {
  border-color: #d32f2f;
}

.error-text {
  color: #d32f2f;
  font-size: 0.875rem;
}

.auth-error {
  color: #d32f2f;
  background-color: #fdecea;
  padding: 0.75rem;
  border-radius: 4px;
  text-align: center;
  margin-bottom: 0.5rem;
}

.auth-button {
  background-color: #2E7D32;
  color: white;
  padding: 0.75rem;
  border: none;
  border-radius: 4px;
  font-size: 1rem;
  font-weight: 500;
  cursor: pointer;
  transition: background-color 0.2s;
}

.auth-button:hover {
  background-color: #1B5E20;
}

.auth-button:disabled {
  background-color: #81C784;
  cursor: not-allowed;
}

.loading-indicator {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}

.auth-footer {
  text-align: center;
  margin-top: 1.5rem;
  color: #666;
}

.auth-link {
  color: #2E7D32;
  font-weight: 500;
  text-decoration: none;
}

.auth-link:hover {
  text-decoration: underline;
}
</style>