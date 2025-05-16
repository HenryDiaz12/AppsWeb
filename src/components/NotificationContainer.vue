<template>
  <div class="notification-container">
    <AppNotification
      v-for="notification in notifications"
      :key="notification.id"
      :type="notification.type"
      :message="notification.message"
      @dismiss="removeNotification(notification.id)"
    />
  </div>
</template>

<script>
import AppNotification from './AppNotification.vue'

export default {
  name: 'NotificationContainer',
  components: {
    AppNotification
  },
  data() {
    return {
      notifications: []
    }
  },
  methods: {
    addNotification(type, message) {
      const id = Date.now()
      this.notifications.push({
        id,
        type,
        message
      })
      
      // Eliminar automáticamente después de 5 segundos
      setTimeout(() => {
        this.removeNotification(id)
      }, 5000)
    },
    removeNotification(id) {
      this.notifications = this.notifications.filter(
        notification => notification.id !== id
      )
    }
  }
}
</script>

<style scoped>
.notification-container {
  position: fixed;
  top: 20px;
  right: 20px;
  z-index: 1000;
  display: flex;
  flex-direction: column;
}
</style>