<script>
import axios from 'axios'

export default {
  props: ['basket', 'removeFromBasket', 'incrementItem', 'decrementItem', 'clearBasket'],
  data() {
    return {
      error: '',
      name: '',
      surname: '',
      email: '',
      phone: '',
      deliveryService: '',
      region: '',
      city: '',
      department: '',
      showForm: false
    }
  },
  computed: {
    getSumma() {
      return this.basket.reduce(
        (total, item) => total + parseFloat(item.price) * (item.count || 1),
        0
      )
    }
  },
  methods: {
    sendData() {
      if (this.name.length < 2) {
        this.error = 'Ім\'я не менше 2 символів'
      } else if (this.surname.length < 2) {
        this.error = 'Прізвище не менше 2 символів'
      } else if (!this.email.includes('@') || !this.email.includes('.')) {
        this.error = 'Email неправильного формату'
      } else if (this.phone.length < 2) {
        this.error = 'Телефон не менше 2 символів'
      } else if (!this.deliveryService) {
        this.error = 'Оберіть службу доставки'
      } else if (!this.region) {
        this.error = 'Вкажіть область'
      } else if (!this.city) {
        this.error = 'Вкажіть місто'
      } else if (!this.department) {
        this.error = 'Вкажіть відділення'
      } else if (this.basket.length === 0 || this.getSumma === 0) {
        this.error = 'Корзина порожня'
      } else {
        this.error = ''
        const data = {
          name: this.name,
          surname: this.surname,
          email: this.email,
          phone: this.phone,
          summa: this.getSumma,
          deliveryService: this.deliveryService,
          region: this.region,
          city: this.city,
          department: this.department,
          basket: JSON.stringify(this.basket)
        }
        axios
          .post('http://127.0.0.1:8000/api/order-add/', data)
          .then(res => {
            this.error = res.data.result || 'Замовлення прийнято!'
            this.clearBasket() // 🧹 очищення корзини
            setTimeout(() => {
              this.$router.push({ name: 'PaymentPage', query: { email: this.email } })
            }, 1000)
          })
      }
    },
    increaseCount(item) {
      this.incrementItem(item.slug)
    },
    decreaseCount(item) {
      this.decrementItem(item.slug)
    }
  }
}
</script>


<template>
  <div class="checkout-page">
    <h1>Корзина</h1>

    <div class="basket-section" v-if="basket.length > 0">
      <div class="item" v-for="el in basket" :key="el.slug">
        <img :src="'/img/' + el.image" :alt="el.title" />
        <div class="info">
          <h3>{{ el.title }}</h3>
          <div class="quantity-controls">
            <button @click="decreaseCount(el)">-</button>
            <span>{{ el.count || 1 }}</span>
            <button @click="increaseCount(el)">+</button>
          </div>
          <span class="price">{{ (parseFloat(el.price) * (el.count || 1)).toFixed(2) }}$</span>
        </div>
        <button class="remove" @click="removeFromBasket(el.slug)">✖</button>
      </div>

      <div class="summary">
        <p>Всього: <strong>{{ getSumma.toFixed(2) }}$</strong></p>
        <button @click="showForm = true" class="order-btn">Оформити</button>
      </div>
    </div>

    <div v-else class="empty">
      <h2>Корзина порожня</h2>
    </div>

    <div v-if="showForm" class="order-form">
      <h2>Оформлення замовлення</h2>
      <form @submit.prevent="sendData">
  <p class="error">{{ error }}</p>
  <h3>Ваші данні</h3>
  <input type="text" v-model="name" placeholder="Ваше ім'я" />
  <input type="text" v-model="surname" placeholder="Ваше прізвище" />
  <input type="email" v-model="email" placeholder="Ваш email" />
  <input type="tel" v-model="phone" placeholder="Телефон" />

  <h3>Адреса доставки</h3>
  <select v-model="deliveryService">
    <option disabled value="">Оберіть службу доставки</option>
    <option>Нова Пошта</option>
    <option>Міст Експрес</option>
    <option>Укрпошта</option>
  </select>

  <input type="text" v-model="region" placeholder="Область" />
  <input type="text" v-model="city" placeholder="Місто" />
  <input type="text" v-model="department" placeholder="Номер відділення" />

  <button type="submit">Купити</button>
</form>
    </div>
  </div>
</template>


<style scoped>

select {
  padding: 10px;
  font-size: 16px;
  border-radius: 6px;
  border: 1px solid #ccc;
}


.checkout-page {
  padding: 30px;
  max-width: 800px;
  margin: auto;
}

h1, h2 {
  text-align: center;
  font-weight: 300;
  margin-bottom: 20px;
}

.basket-section {
  margin-bottom: 30px;
}

.item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-bottom: 1px solid #eee;
  padding: 15px 0;
  gap: 10px;
}

.item img {
  width: 70px;
  height: 70px;
  border-radius: 8px;
  object-fit: cover;
}

.info {
  flex: 1;
}

.quantity-controls {
  display: flex;
  align-items: center;
  gap: 5px;
  margin: 10px 0;
}

.quantity-controls button {
  width: 30px;
  height: 30px;
  background-color: #f0f0f0;
  border: none;
  border-radius: 4px;
  font-size: 18px;
  cursor: pointer;
}

.price {
  font-weight: bold;
}

.remove {
  background: transparent;
  border: none;
  font-size: 20px;
  cursor: pointer;
  color: #c00;
}

.summary {
  text-align: center;
  margin-top: 20px;
}

.order-btn {
  background-color: #2a7f62;
  color: white;
  padding: 12px 30px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 18px;
  transition: background-color 0.3s ease;
}

.order-btn:hover {
  background-color: #1f5d4a;
}

.order-form form {
  display: flex;
  flex-direction: column;
  gap: 10px;
  max-width: 400px;
  margin: 20px auto;
}

input {
  padding: 12px;
  font-size: 16px;
  border-radius: 6px;
  border: 1px solid #ccc;
}

button[type='submit'] {
  padding: 12px;
  font-size: 18px;
  background-color: #216f5b;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
}

button[type='submit']:hover {
  background-color: #184e42;
}

.error {
  color: red;
  text-align: center;
  margin-bottom: 10px;
}

.empty {
  text-align: center;
  margin-top: 50px;
}

</style>
