<template>
  <div class="dashboard-view">
    <h1>Bienvenido </h1>
    
    <div class="stats-grid">
      <div class="stat-card">
        <h3>Productos Registrados</h3>
        <p class="stat-value">{{ productCount }}</p>
      </div>
      <div class="stat-card">
        <h3>Productores Activos</h3>
        <p class="stat-value">{{ activeProducers }}</p>
      </div>
    </div>
    
    <div class="quick-actions">
      <router-link to="/registrar-producto" class="action-btn">
        <i class="fas fa-plus"></i> Nuevo Producto
      </router-link>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DashboardView',
  data() {
    return {
      productCount: 24,
      activeProducers: 8
    }
  },
  mounted() {
    this.$root.showNotification('success', 'Sesión iniciada correctamente')
  }
}
</script>

<style scoped>
.dashboard-view {
  max-width: 1200px;
  margin: 0 auto;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1.5rem;
  margin: 2rem 0;
}

.stat-card {
  background: white;
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.stat-value {
  font-size: 2rem;
  font-weight: bold;
  color: var(--primary-color);
  margin-top: 0.5rem;
}

.quick-actions {
  margin-top: 2rem;
}

.action-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem 1.5rem;
  background-color: var(--primary-color);
  color: white;
  text-decoration: none;
  border-radius: 4px;
  transition: background-color 0.2s;
}

.action-btn:hover {
  background-color: var(--primary-dark);
}
</style>