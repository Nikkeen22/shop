<script>
import Header from '../components/Header.vue'
import OrderForm from '../components/OrderForm.vue'

export default {
  props: ['basket', 'removeFromBasket', 'incrementItem', 'decrementItem', 'clearBasket'],
  components: { Header, OrderForm }
}
</script>

<template>
  <div>
      <Header
        :basket="basket"
        :removeFromBasket="removeFromBasket"
        :incrementItem="incrementItem"
        :decrementItem="decrementItem"
      />
      <OrderForm
        :basket="basket"
        :removeFromBasket="removeFromBasket"
        :incrementItem="incrementItem"
        :decrementItem="decrementItem"
        :clearBasket="clearBasket"
      />  

      <MainSection />
  </div>
</template>
