<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import { useRoute, useRouter, RouterLink } from 'vue-router'
import Header from '../components/Header.vue'

const route = useRoute()
const router = useRouter()
const email = ref(route.query.email || '')
const message = ref('')
const loading = ref(false)
const confirmed = ref(false) // стан підтвердження

const props = defineProps(['basket', 'removeFromBasket', 'incrementItem', 'decrementItem', 'toggleMenu'])

const sendInvoice = async () => {
  if (loading.value || confirmed.value) return; // Блокуємо повторний запуск

  if (!email.value) {
    message.value = 'Будь ласка, введіть email.'
    return
  }

  loading.value = true
  message.value = ''

  try {
    await axios.post('https://my-unique-shop-backend-c2411867a870.herokuapp.com/api/payment-await/', {
      email: email.value,
    })
    message.value = 'Чек надіслано. Очікуйте підтвердження оплати.'
    confirmed.value = true
  } catch (error) {
    message.value = 'Помилка при надсиланні чеку.'
    if (error.response && error.response.data) {
      message.value += ` Деталі: ${JSON.stringify(error.response.data)}`
    }
  } finally {
    loading.value = false
  }
}

const goHomeWithToggle = () => {
  if (props.toggleMenu) props.toggleMenu()
  router.push({ name: 'HomePage' }) // або to: '/'
}
</script>

<template>
  <Header
    :basket="basket"
    :removeFromBasket="removeFromBasket"
    :incrementItem="incrementItem"
    :decrementItem="decrementItem"
  />
  <div class="payment-page">
    <h1 class="title">Оплата</h1>
    <div class="payment-info">
      <p>Реквізити для оплати:</p>
      <p><strong>Номер картки:</strong> 1234 5678 9012 3456</p>
      <p class="amount">
        Після того як ви підтвердите оплату, ми опрацюємо ваше замовлення та вам прийде лист на пошту
      </p>
    </div>

    <button
      v-if="!confirmed"
      class="btn-confirm"
      @click="sendInvoice"
      :disabled="loading"
    >
      <span v-if="loading">Надсилання...</span>
      <span v-else>Підтвердити</span>
    </button>

    <RouterLink
      v-else
      to="/"
      class="btn-confirm"
      @click.native.prevent="goHomeWithToggle"
    >
      На головну
    </RouterLink>

    <p v-if="message" class="message">{{ message }}</p>
  </div>
</template>





<style scoped>



.payment-page {
  max-width: 400px;
  margin: 40px auto;
  padding: 20px 25px;
  background: #fff;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  text-align: center;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  color: #2f3336;
}

.title {
  font-weight: 700;
  margin-bottom: 20px;
  color: #216F5B;
}

.payment-info p {
  margin: 8px 0;
  font-size: 16px;
}

.amount {
  font-size: 18px;
  margin-top: 12px;
  color: #184e42;
  font-weight: 600;
}

.input-email {
  width: 100%;
  padding: 10px 12px;
  margin: 20px 0 10px;
  font-size: 16px;
  border: 1.5px solid #ccc;
  border-radius: 8px;
  transition: border-color 0.3s ease;
}

.input-email:focus {
  border-color: #216F5B;
  outline: none;
}

.btn-confirm {
  display: block;
  width: 100%;
  padding: 12px 0;
  background-color: #216F5B;
  border: none;
  border-radius: 10px;
  color: #fff;
  font-weight: 600;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  text-align: center; /* щоб текст був по центру */
  text-decoration: none; /* прибрати підкреслення */
  user-select: none;
}


.btn-confirm:hover {
  background-color: #184e42;
}

.message {
  margin-top: 15px;
  font-size: 15px;
  color: #2f3336;
  font-weight: 500;
}
</style>
