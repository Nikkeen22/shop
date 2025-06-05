<script>
import Header from '../components/Header.vue'
import HeadSection from '../components/HeadSection.vue'
import ItemsSection from '@/components/ItemsSection.vue';

export default {
    props: ['basket', 'addToBasket', 'removeFromBasket', 'incrementItem', 'decrementItem'],
    components: { Header, HeadSection, ItemsSection }
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

      <HeadSection />
      <ItemsSection :addToBasket="addToBasket" />
  </div>
</template>
