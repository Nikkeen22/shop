<script>
import Header from '../components/Header.vue'
import MainSection from '../components/MainSection.vue'

export default {
  props: ['basket', 'removeFromBasket', 'incrementItem', 'decrementItem'],
  components: { Header, MainSection }
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
      <MainSection />
  </div>
</template>
