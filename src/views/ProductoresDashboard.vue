<template>
  <div class="transport-request">
    <header class="request-header">
      <h1>Solicitud de Transporte</h1>
      <p>Complete el formulario para solicitar el transporte de sus productos</p>
    </header>
    
    <main class="request-content">
      <div class="transport-form">
        <form @submit.prevent="submitTransportRequest">
          <div class="form-group">
            <label>Producto a Transportar</label>
            <select v-model="transportData.productId" required>
              <option value="" disabled>Seleccione un producto</option>
              <option v-for="product in products" :key="product.id" :value="product.id">
                {{ product.name }} ({{ product.quantity }} {{ product.unit }})
              </option>
            </select>
          </div>
          
          <div class="form-group">
            <label>Cantidad</label>
            <input type="number" v-model="transportData.quantity" required min="1">
          </div>
          
          <div class="form-group">
            <label>Origen</label>
            <input type="text" v-model="transportData.origin" required>
          </div>
          
          <div class="form-group">
            <label>Destino</label>
            <input type="text" v-model="transportData.destination" required>
          </div>
          
          <div class="form-group">
            <label>Fecha Requerida</label>
            <input type="date" v-model="transportData.requiredDate" required>
          </div>
          
          <div class="form-group">
            <label>Instrucciones Especiales (Opcional)</label>
            <textarea v-model="transportData.specialInstructions"></textarea>
          </div>
          
          <div class="form-actions">
            <button type="submit" class="submit-btn">
              Solicitar Transporte
            </button>
          </div>
        </form>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const transportData = ref({
  productId: '',
  quantity: 1,
  origin: '',
  destination: '',
  requiredDate: '',
  specialInstructions: ''
})

const products = ref([
  { id: 1, name: 'Manzanas', quantity: 100, unit: 'kg' },
  { id: 2, name: 'Naranjas', quantity: 150, unit: 'kg' },
  { id: 3, name: 'Lechugas', quantity: 50, unit: 'unidades' }
])

const submitTransportRequest = () => {

  console.log('Solicitud enviada:', transportData.value)
  alert('Solicitud de transporte enviada con éxito')
  
 
  transportData.value = {
    productId: '',
    quantity: 1,
    origin: '',
    destination: '',
    requiredDate: '',
    specialInstructions: ''
  }
}
</script>

<style scoped>
.transport-request {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.request-header {
  text-align: center;
  margin-bottom: 2rem;
}

.request-header h1 {
  color: #2E7D32;
  margin-bottom: 0.5rem;
}

.request-header p {
  color: #666;
  margin: 0;
}

.transport-form {
  background-color: #FFFFFF;
  border-radius: 8px;
  padding: 2rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.form-group {
  margin-bottom: 1.5rem;
}

.form-group label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: 500;
  color: #333;
}

.form-group input,
.form-group select,
.form-group textarea {
  width: 100%;
  padding: 0.75rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 1rem;
  transition: border-color 0.2s;
}

.form-group input:focus,
.form-group select:focus,
.form-group textarea:focus {
  border-color: #2E7D32;
  outline: none;
}

.form-group textarea {
  min-height: 100px;
  resize: vertical;
}

.form-actions {
  display: flex;
  justify-content: center;
  margin-top: 2rem;
}

.submit-btn {
  background-color: #2E7D32;
  color: white;
  border: none;
  padding: 0.75rem 2rem;
  border-radius: 4px;
  font-size: 1rem;
  font-weight: 500;
  cursor: pointer;
  transition: background-color 0.2s;
}

.submit-btn:hover {
  background-color: #388E3C;
}

@media (max-width: 768px) {
  .transport-request {
    padding: 1rem;
  }
  
  .transport-form {
    padding: 1.5rem;
  }
}
</style>