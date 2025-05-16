<template>
  <div class="product-registration">
    <div class="registration-card">
      <h1 class="registration-title">Registrar Nuevo Producto</h1>
      
      <form @submit.prevent="handleSubmit" class="registration-form">

        <div class="form-group">
          <label for="Greenlogist" class="form-label">Nombre del Producto</label>
          <input
            type="text"
            id="Greenlogist"
            v-model.trim="product.name"
            required
            class="form-input"
            :class="{ 'input-error': errors.name }"
            placeholder="Ej: Manzanas orgánicas"
          />
          <span v-if="errors.name" class="error-text">{{ errors.name }}</span>
        </div>


        <div class="form-row">
          <div class="form-group">
            <label for="quantity" class="form-label">Cantidad</label>
            <input
              type="number"
              id="quantity"
              v-model.number="product.quantity"
              min="0"
              step="1"
              required
              class="form-input"
              :class="{ 'input-error': errors.quantity }"
            />
            <span v-if="errors.quantity" class="error-text">{{ errors.quantity }}</span>
          </div>

          <div class="form-group">
            <label for="unit" class="form-label">Unidad</label>
            <select
              id="unit"
              v-model="product.unit"
              required
              class="form-input"
              :class="{ 'input-error': errors.unit }"
            >
              <option value="" disabled>Seleccione...</option>
              <option value="kg">Kilogramos (kg)</option>
              <option value="g">Gramos (g)</option>
              <option value="lb">Libras (lb)</option>
              <option value="unidades">Unidades</option>
              <option value="litros">Litros (L)</option>
              <option value="cajas">Cajas</option>
            </select>
            <span v-if="errors.unit" class="error-text">{{ errors.unit }}</span>
          </div>
        </div>


        <div class="form-group">
          <label for="price" class="form-label">Precio por unidad</label>
          <div class="price-input-container">
            <span class="currency-symbol">S/</span>
            <input
              type="number"
              id="price"
              v-model.number="product.price"
              min="0"
              step="0.01"
              required
              class="form-input"
              :class="{ 'input-error': errors.price }"
              placeholder="0.00"
            />
          </div>
          <span v-if="errors.price" class="error-text">{{ errors.price }}</span>
        </div>

 
        <div class="form-group">
          <label for="category" class="form-label">Categoría</label>
          <select
            id="category"
            v-model="product.category"
            required
            class="form-input"
            :class="{ 'input-error': errors.category }"
          >
            <option value="" disabled>Seleccione...</option>
            <option value="frutas">Frutas</option>
            <option value="verduras">Verduras</option>
            <option value="lacteos">Lácteos</option>
            <option value="carnes">Carnes</option>
            <option value="granos">Granos</option>
            <option value="otros">Otros</option>
          </select>
          <span v-if="errors.category" class="error-text">{{ errors.category }}</span>
        </div>

 
        <div class="form-group">
          <label for="description" class="form-label">Descripción</label>
          <textarea
            id="description"
            v-model.trim="product.description"
            required
            class="form-input"
            :class="{ 'input-error': errors.description }"
            rows="4"
            placeholder="Detalles sobre el producto, calidad, origen, etc."
          ></textarea>
          <span v-if="errors.description" class="error-text">{{ errors.description }}</span>
        </div>

        <!-- Imagen del Producto -->
        <div class="form-group">
          <label class="form-label">Imagen del Producto</label>
          <div class="image-upload-container">
            <input
              type="file"
              id="productImage"
              accept="image/*"
              @change="handleImageUpload"
              class="image-upload-input"
            />
            <label for="productImage" class="image-upload-label">
              <i class="fas fa-cloud-upload-alt"></i>
              <span v-if="!product.image">Seleccionar imagen</span>
              <span v-else>Cambiar imagen</span>
            </label>
            <div v-if="product.image" class="image-preview">
              <img :src="product.imagePreview" alt="Vista previa" class="preview-image" />
              <button type="button" @click="removeImage" class="remove-image-btn">
                <i class="fas fa-times"></i>
              </button>
            </div>
          </div>
        </div>

        <div v-if="submitError" class="submit-error">
          {{ submitError }}
        </div>

        <!-- Botones -->
        <div class="form-actions">
          <button type="submit" class="submit-btn" :disabled="isSubmitting">
            <span v-if="!isSubmitting">Registrar Producto</span>
            <span v-else class="loading-indicator">
              <i class="fas fa-spinner fa-spin"></i> Procesando...
            </span>
          </button>
          
          <router-link to="/dashboard" class="cancel-btn">
            Cancelar
          </router-link>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

// Datos del producto
const product = reactive({
  name: '',
  quantity: null,
  unit: '',
  price: null,
  category: '',
  description: '',
  image: null,
  imagePreview: ''
})

// Estados
const isSubmitting = ref(false)
const submitError = ref('')
const errors = reactive({
  name: '',
  quantity: '',
  unit: '',
  price: '',
  category: '',
  description: ''
})
 //////validaciones

const handleImageUpload = (event) => {
  const file = event.target.files[0]
  if (file) {
    if (file.size > 2 * 1024 * 1024) { // 2MB
      submitError.value = 'La imagen no debe exceder 2MB'
      return
    }
    
    product.image = file
    product.imagePreview = URL.createObjectURL(file)
    submitError.value = ''
  }
}


const removeImage = () => {
  product.image = null
  product.imagePreview = ''
}


const validateForm = () => {
  let isValid = true
  Object.keys(errors).forEach(key => { errors[key] = '' })


  if (!product.name.trim()) {
    errors.name = 'El nombre es obligatorio'
    isValid = false
  } else if (product.name.trim().length < 3) {
    errors.name = 'Mínimo 3 caracteres'
    isValid = false
  }


  if (product.quantity === null || product.quantity === '') {
    errors.quantity = 'La cantidad es obligatoria'
    isValid = false
  } else if (product.quantity < 0) {
    errors.quantity = 'La cantidad no puede ser negativa'
    isValid = false
  }

  if (!product.unit) {
    errors.unit = 'Seleccione una unidad'
    isValid = false
  }


  if (product.price === null || product.price === '') {
    errors.price = 'El precio es obligatorio'
    isValid = false
  } else if (product.price < 0) {
    errors.price = 'El precio no puede ser negativo'
    isValid = false
  }


  if (!product.category) {
    errors.category = 'Seleccione una categoría'
    isValid = false
  }

  if (!product.description.trim()) {
    errors.description = 'La descripción es obligatoria'
    isValid = false
  } else if (product.description.trim().length < 10) {
    errors.description = 'Mínimo 10 caracteres'
    isValid = false
  }

  return isValid
}
////////////////fin validaciones////////////////
//formulario
const handleSubmit = async () => {
  if (!validateForm()) return

  isSubmitting.value = true
  submitError.value = ''

  try {
//alcenamiento simulado
    const products = JSON.parse(localStorage.getItem('products')) || []
    
    const newProduct = {
      id: Date.now().toString(),
      name: product.name,
      quantity: product.quantity,
      unit: product.unit,
      price: product.price,
      category: product.category,
      description: product.description,
      image: product.image ? await toBase64(product.image) : null,
      createdAt: new Date().toISOString()
    }

    products.push(newProduct)
    localStorage.setItem('products', JSON.stringify(products))


    router.push({
      path: '/dashboard',
      query: { productRegistered: 'true' }
    })
  } catch (error) {
    console.error('Error al registrar producto:', error)
    submitError.value = 'Ocurrió un error al registrar el producto. Intente nuevamente.'
  } finally {
    isSubmitting.value = false
  }
}

const toBase64 = (file) => new Promise((resolve, reject) => {
  const reader = new FileReader()
  reader.readAsDataURL(file)
  reader.onload = () => resolve(reader.result)
  reader.onerror = error => reject(error)
})
</script>

<style scoped>
.product-registration {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  min-height: 100vh;
  padding: 2rem 1rem;
  background-color: #f5f5f5;
}

.registration-card {
  background: white;
  border-radius: 12px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
  padding: 2rem;
  width: 100%;
  max-width: 700px;
}

.registration-title {
  color: #2E7D32;
  text-align: center;
  margin-bottom: 1.5rem;
  font-size: 1.5rem;
}

.registration-form {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.form-label {
  font-weight: 500;
  color: #333;
  font-size: 0.9rem;
}

.form-input {
  padding: 0.75rem;
  border: 1px solid #ddd;
  border-radius: 6px;
  font-size: 1rem;
  transition: border-color 0.2s;
}

.form-input:focus {
  outline: none;
  border-color: #2E7D32;
  box-shadow: 0 0 0 2px rgba(46, 125, 50, 0.1);
}

.input-error {
  border-color: #d32f2f;
}

.error-text {
  color: #d32f2f;
  font-size: 0.8rem;
}

.price-input-container {
  position: relative;
}

.currency-symbol {
  position: absolute;
  left: 0.75rem;
  top: 50%;
  transform: translateY(-50%);
  color: #666;
}

.price-input-container .form-input {
  padding-left: 1.5rem;
}

textarea.form-input {
  resize: vertical;
  min-height: 100px;
}

/* Estilos para subida de imagen */
.image-upload-container {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.image-upload-input {
  display: none;
}

.image-upload-label {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  padding: 0.75rem;
  background-color: #f5f5f5;
  border: 1px dashed #ccc;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.2s;
}

.image-upload-label:hover {
  background-color: #eee;
  border-color: #999;
}

.image-preview {
  position: relative;
  margin-top: 0.5rem;
}

.preview-image {
  width: 100%;
  max-height: 200px;
  object-fit: contain;
  border-radius: 6px;
  border: 1px solid #eee;
}

.remove-image-btn {
  position: absolute;
  top: 0.5rem;
  right: 0.5rem;
  background-color: rgba(211, 47, 47, 0.8);
  color: white;
  border: none;
  border-radius: 50%;
  width: 1.5rem;
  height: 1.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

/* Mensaje de error */
.submit-error {
  color: #d32f2f;
  background-color: #fdecea;
  padding: 0.75rem;
  border-radius: 6px;
  text-align: center;
  margin: 0.5rem 0;
}

/* Botones */
.form-actions {
  display: flex;
  gap: 1rem;
  margin-top: 1rem;
}

.submit-btn {
  flex: 1;
  background-color: #2E7D32;
  color: white;
  padding: 0.75rem;
  border: none;
  border-radius: 6px;
  font-size: 1rem;
  font-weight: 500;
  cursor: pointer;
  transition: background-color 0.2s;
}

.submit-btn:hover {
  background-color: #1B5E20;
}

.submit-btn:disabled {
  background-color: #81C784;
  cursor: not-allowed;
}

.cancel-btn {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0.75rem;
  border: 1px solid #ddd;
  border-radius: 6px;
  color: #333;
  text-decoration: none;
  transition: all 0.2s;
}

.cancel-btn:hover {
  background-color: #f5f5f5;
  border-color: #999;
}

.loading-indicator {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}


@media (max-width: 600px) {
  .form-row {
    grid-template-columns: 1fr;
  }
  
  .form-actions {
    flex-direction: column;
  }
}
</style>