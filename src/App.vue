<template>
  <div id="app">
    <div class="app-container">
      <aside class="app-sidebar" v-if="showSidebar">
        <div class="sidebar-header">
          <h2>GreenLogist</h2>
          <p>Panel de Control</p>
        </div>

        <nav class="sidebar-nav">
          <router-link to="/dashboard">
            <i class="fas fa-home"></i> Dashboard
          </router-link>
          <router-link to="/registrar-producto">
            <i class="fas fa-box"></i> Registrar Producto
          </router-link>
          <router-link to="/productores">
            <i class="fas fa-users"></i> Solicitud de Transporte
          </router-link>
          <router-link to="/estadisticas">
            <i class="fas fa-chart-bar"></i> Estadísticas
          </router-link>
          <router-link to="/perfil">
            <i class="fas fa-user"></i> Mi Perfil
          </router-link>
        </nav>

        <!-- Botón de cierre de sesión CORREGIDO -->
        <button class="logout-btn" @click="handleLogout">
          <i class="fas fa-sign-out-alt"></i> Cerrar Sesión
        </button>
      </aside>

      <main class="app-content">
        <router-view />
      </main>
    </div>

    <NotificationContainer ref="notificationContainer" />
  </div>
</template>

<script>
import NotificationContainer from '@/components/NotificationContainer.vue'

export default {
  name: 'App',
  components: {
    NotificationContainer
  },
  methods: {
    handleLogout() {
      // 1. Limpiar datos de sesión
      localStorage.removeItem('currentUser')
      
      // 2. Redirigir a login
      this.$router.push('/login')
      
      // 3. Mostrar notificación
      this.showNotification('success', 'Sesión cerrada correctamente')
    },
    showNotification(type, message, duration = 5000) {
      this.$refs.notificationContainer.addNotification({
        type,
        message,
        duration
      })
    }
  },
  computed: {
    showSidebar() {
      return !['/login', '/register'].includes(this.$route.path)
    }
  },
  provide() {
    return {
      showNotification: this.showNotification
    }
  }
}
</script>

<style>

:root {
  --primary-color: #2E7D32;
  --primary-dark: #1B5E20;
  --primary-light: #81C784;
  --sidebar-width: 250px;
  --header-height: 60px;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #f5f5f5;
}

#app {
  min-height: 100vh;
}


.app-container {
  display: flex;
  min-height: 100vh;
}


.app-sidebar {
  width: var(--sidebar-width);
  background-color: white;
  box-shadow: 2px 0 10px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  padding: 1rem;
}

.sidebar-header {
  padding: 1rem;
  margin-bottom: 2rem;
  border-bottom: 1px solid #eee;
}

.sidebar-header h2 {
  color: var(--primary-color);
  margin-bottom: 0.5rem;
}

.sidebar-nav {
  flex-grow: 1;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.sidebar-nav a {
  padding: 0.75rem 1rem;
  border-radius: 4px;
  color: #333;
  text-decoration: none;
  display: flex;
  align-items: center;
  gap: 0.75rem;
  transition: all 0.2s;
}

.sidebar-nav a:hover {
  background-color: #f0f0f0;
}

.sidebar-nav a.router-link-exact-active {
  background-color: var(--primary-light);
  color: var(--primary-dark);
  font-weight: 500;
}

.logout-btn {
  margin-top: auto;
  padding: 0.75rem;
  background: none;
  border: none;
  color: #D32F2F;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 0.75rem;
  border-radius: 4px;
}

.logout-btn:hover {
  background-color: #ffebee;
}

/* Contenido Principal */
.app-content {
  flex: 1;
  padding: 2rem;
  background-color: #f9f9f9;
}

/* Responsive */
@media (max-width: 768px) {
  .app-sidebar {
    width: 70px;
    padding: 0.5rem;
  }
  
  .sidebar-nav a span, 
  .sidebar-header p,
  .logout-btn span {
    display: none;
  }
  
  .sidebar-nav a,
  .logout-btn {
    justify-content: center;
  }
}
</style>