<script>
import { RouterView } from 'vue-router';

export default {
  data() {
    return {
      isOpen: false,
      showBasket: false,
      basket: [],

      // Поля для сповіщення
      notificationMessage: '',
      showNotification: false,
    }
  },
  methods: {
    addToBasket(item) {
      const existingItem = this.basket.find(el => el.slug === item.slug);
      if (existingItem) {
        existingItem.count++;
      } else {
        this.basket.push({ ...item, count: 1 });
      }
      this.saveBasket();

      // Викликаємо сповіщення
      this.notify(`${item.name || item.title || 'Товар'} додано в корзину`);
    },
    removeFromBasket(slug) {
      this.basket = this.basket.filter(item => item.slug !== slug);
      this.saveBasket();
    },
    incrementItem(slug) {
      const item = this.basket.find(el => el.slug === slug);
      if (item) item.count++;
      this.saveBasket();
    },
    decrementItem(slug) {
      const item = this.basket.find(el => el.slug === slug);
      if (item && item.count > 1) {
        item.count--;
      } else {
        this.removeFromBasket(slug);
      }
      this.saveBasket();
    },
    clearBasket() {
      this.basket = []
      this.saveBasket()
    },
    saveBasket() {
      localStorage.setItem("basket", JSON.stringify(this.basket));
    },

    // Метод для показу toast‑сповіщення
    notify(message) {
      this.notificationMessage = message;
      this.showNotification = true;

      // Через 2.5 с ховаємо сповіщення
      setTimeout(() => {
        this.showNotification = false;
      }, 2500);
    }
  },

  created() {
    const stored = localStorage.getItem("basket");
    if (stored) {
      this.basket = JSON.parse(stored);
    }
  }
}
</script>

<template>
  <div class="container">
    <RouterView v-slot="{ Component }">
      <component
        :is="Component"
        :basket="basket"
        :addToBasket="addToBasket"
        :removeFromBasket="removeFromBasket"
        :incrementItem="incrementItem"
        :decrementItem="decrementItem"
        :clearBasket="clearBasket"
      />
    </RouterView>

    <!-- Toast-повідомлення -->
    <transition name="fade">
      <div v-if="showNotification" class="toast-notification">
        {{ notificationMessage }}
      </div>
    </transition>
  </div>
</template>

<style scoped>


.toast-notification {
  position: fixed;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  background-color: rgba(76, 160, 114, 0.8);
  color: #000000;
  padding: 12px 20px;
  border-radius: 4px;
  font-size: 14px;
  z-index: 9999;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.3);
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.3s;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
.fade-enter-to, .fade-leave-from {
  opacity: 1;
}
</style>