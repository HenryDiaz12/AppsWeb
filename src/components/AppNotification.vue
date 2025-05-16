<template>
  <div :class="['app-notification', type]">
    <span class="notification-content">{{ message }}</span>
    <button class="dismiss-btn" @click="handleDismiss" aria-label="Cerrar notificación">
      &times;
    </button>
  </div>
</template>

<script>
export default {
  name: 'AppNotification',
  props: {
    type: {
      type: String,
      default: 'info',
      validator: (value) => ['info', 'success', 'error', 'warning'].includes(value)
    },
    message: {
      type: String,
      required: true
    }
  },
  emits: ['dismiss'],
  methods: {
    handleDismiss() {
      this.$emit('dismiss')
    }
  }
}
</script>

<style scoped>
.app-notification {
  padding: 12px 16px;
  margin-bottom: 12px;
  border-radius: 4px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
  max-width: 400px;
}

.notification-content {
  flex-grow: 1;
  padding-right: 12px;
}

.dismiss-btn {
  background: none;
  border: none;
  font-size: 1.5rem;
  cursor: pointer;
  color: inherit;
  padding: 0 4px;
  line-height: 1;
}

.app-notification.info {
  background-color: #e7f5ff;
  color: #1c7ed6;
  border-left: 4px solid #1c7ed6;
}

.app-notification.success {
  background-color: #ebfbee;
  color: #2b8a3e;
  border-left: 4px solid #2b8a3e;
}

.app-notification.error {
  background-color: #fff5f5;
  color: #c92a2a;
  border-left: 4px solid #c92a2a;
}

.app-notification.warning {
  background-color: #fff9db;
  color: #e67700;
  border-left: 4px solid #e67700;
}
</style>