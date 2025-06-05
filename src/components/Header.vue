<script>
export default {
  props: ['basket', 'removeFromBasket', 'incrementItem', 'decrementItem'],
  data() {
    return {
      isOpen: false,
      showBasket: false
    };
  },
  computed: {
    totalCount() {
      return this.basket.reduce((sum, item) => sum + item.count, 0);
    },
    totalPrice() {
      return this.basket.reduce((sum, item) => sum + item.count * item.price, 0);
    }
  },
  methods: {
    toggleMenu() {
      this.isOpen = !this.isOpen;
    }
  }
}
</script>

<template>
  <header>
    <img src="/img/logo.svg" alt="Logo" class="logo" />

    <button class="burger" @click="toggleMenu" aria-label="Toggle menu">
      <span :class="{ open: isOpen }"></span>
      <span :class="{ open: isOpen }"></span>
      <span :class="{ open: isOpen }"></span>
    </button>

    <ul :class="{ open: isOpen }">
      <li><RouterLink to="/" active-class="active" @click="toggleMenu">Головна</RouterLink></li>
      <li><RouterLink to="/items" active-class="active" @click="toggleMenu">Товари</RouterLink></li>
      <li><RouterLink to="/delivery" active-class="active" @click="toggleMenu">Доставка</RouterLink></li>
      <li><RouterLink to="/about" active-class="active" @click="toggleMenu">Про нас</RouterLink></li>
      <li><img src="/img/purchase.svg" alt="Purchase" @click="showBasket = !showBasket" class="purchase-icon" /></li>
    </ul>

    <div class="basket" v-if="showBasket">
      <button class="close-button" @click="showBasket = false">×</button>

      <h3 class="basket-title" v-if="basket.length == 0">Товарів немає</h3>
      <div class="items" v-else>
        <h3 class="basket-title">Ваші товари</h3>
        <div class="item" v-for="el in basket" :key="el.slug">
          <img :src="'/img/' + el.image" :alt="el.title" />
          <div class="item-info">
            <h4>{{ el.title }}</h4>
            <span>{{ el.price }} $</span>
            <div class="quantity">
              <button @click="decrementItem(el.slug)">-</button>
              <span>{{ el.count }}</span>
              <button @click="incrementItem(el.slug)">+</button>
            </div>
          </div>
          <button class="remove-btn" @click="removeFromBasket(el.slug)" aria-label="Видалити товар">
            &times;
          </button>
        </div>

        <div class="basket-summary">
          <p>Кількість: {{ totalCount }}</p>
          <p>Сума: {{ totalPrice.toFixed(2) }} $</p>
        </div>
        <RouterLink to="/order"><button class="checkout-btn">Перейти до оплати</button></RouterLink>
      </div>
    </div>
  </header>
</template>

<style scoped>
.basket {
  position: fixed;
  top: 70px;
  right: 20px;
  width: 320px;
  max-height: 80vh;
  overflow-y: auto;
  background: #ffffff;
  border: 1px solid #ccc;
  border-radius: 12px;
  padding: 20px;
  z-index: 1000;
  box-shadow: 0px 4px 12px rgba(0,0,0,0.15);
  transition: transform 0.3s ease;
}

.close-button {
  position: absolute;
  top: 10px;
  right: 15px;
  background: none;
  border: none;
  font-size: 22px;
  font-weight: bold;
  color: #444;
  cursor: pointer;
}

.basket-title {
  margin-top: 0;
  color: #2f3336;
  font-size: 18px;
  font-weight: 600;
  text-align: center;
}

.basket .item {
  display: flex;
  align-items: center; 
  gap: 12px;
  margin-bottom: 15px;
  border-bottom: 1px solid #eee;
  padding-bottom: 10px;
}

.basket .item img {
  width: 60px;
  height: 60px;
  object-fit: cover;
  border-radius: 8px;
}


.basket .item-info {
  flex-grow: 1; 
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.basket .item-info h4 {
  margin: 0;
  font-size: 16px;
  color: #333;
}

.basket .item-info span {
  font-weight: 500;
  color: #777;
  margin-top: 4px;
}


.quantity {
  display: flex;
  align-items: center;
  gap: 5px;
  margin-top: 5px;
}

.quantity button {
  width: 25px;
  height: 25px;
  font-size: 18px;
  background: #eee;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.remove-btn {
  background: #e63946;
  color: white;
  border: none;
  border-radius: 50%;
  width: 24px;
  height: 24px;
  font-size: 18px;
  line-height: 20px;
  font-weight: bold;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0;
  user-select: none;
  transition: background-color 0.3s ease;
  flex-shrink: 0; 
}

.remove-btn:hover {
  background: #d62828;
}

.remove-btn:focus {
  outline: 2px solid #f1faee;
  outline-offset: 2px;
}

.basket-summary {
  border-top: 1px solid #ddd;
  padding-top: 15px;
  margin-top: 10px;
  font-weight: 600;
  color: #333;
}

.checkout-btn {
  margin-top: 15px;
  width: 100%;
  padding: 12px;
  background: #216F5B;
  color: #fff;
  font-size: 16px;
  font-weight: bold;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.3s ease;
}

.checkout-btn:hover {
  background: #184e42;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 50px;
  padding: 0 20px;
  position: relative;
  flex-wrap: wrap;
}

.logo {
  max-width: 150px;
  height: auto;
}

header ul {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  transition: max-height 0.3s ease;
}

header ul li {
  display: inline-block;
  margin-left: 25px;
  cursor: pointer;
  white-space: nowrap;
}

header ul li:first-child {
  margin-left: 0;
}

header ul li .active {
  padding: 5px 15px;
  background: #B2BAC1;
  border-radius: 20px;
  transition: background-color 0.3s, color 0.3s;
}

header ul li .active:hover {
  color: #fff;
  background: #616569;
}

header ul li a {
  position: relative;
  bottom: 10px;
  text-decoration: none;
  color: inherit;
}

.purchase-icon {
  width: 24px;
  height: 24px;
  margin-left: 25px;
}

.burger {
  display: none;
  flex-direction: column;
  justify-content: space-between;
  width: 25px;
  height: 20px;
  background: transparent;
  border: none;
  cursor: pointer;
  padding: 0;
  z-index: 999;
}

.burger span {
  display: block;
  height: 3px;
  width: 100%;
  background: #216F5B;
  border-radius: 2px;
  transition: all 0.3s ease;
}

.burger span.open:nth-child(1) {
  transform: rotate(45deg) translate(5px, 5px);
}

.burger span.open:nth-child(2) {
  opacity: 0;
}

.burger span.open:nth-child(3) {
  transform: rotate(-45deg) translate(5px, -5px);
}

@media (max-width: 768px) {
  .burger {
    display: flex;
  }

  header ul {
    position: absolute;
    top: 70px;
    left: 0;
    right: 0;
    background: white;
    flex-direction: column;
    max-height: 0;
    overflow: hidden;
  }

  header ul.open {
    max-height: 300px;
    border-top: 1px solid #ddd;
  }

  header ul li {
    margin: 15px 0;
    text-align: center;
  }
}
</style>
