<template>
  <div class="payment-wait">
    <h1>Очікуйте...</h1>
    <p>Ви зараз будете перенаправлені на сторінку оплати.</p>
  </div>
</template>

<script setup>
import { onMounted } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

onMounted(() => {
  setTimeout(() => {
    router.push('/payment')
  }, 3000)
})
</script>

<style scoped>
.payment-wait {
  text-align: center;
  margin-top: 50px;
}
</style>
