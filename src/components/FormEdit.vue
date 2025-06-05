<script setup>
import { ref } from 'vue'
import axios from 'axios'
import { useRouter } from 'vue-router'

const props = defineProps({
  slug: String,
  initialTitle: String,
  initialPrice: Number,
  initialImage: String
})

const showForm = ref(false)
const title = ref(props.initialTitle)
const price = ref(props.initialPrice)
const image = ref(props.initialImage)
const message = ref('')

const router = useRouter()

const toggleForm = () => {
  showForm.value = !showForm.value
}

const updateItem = async () => {
  try {
    await axios.put(`https://my-unique-shop-backend-c2411867a870.herokuapp.com/api/edit-item/${props.slug}`, {
      title: title.value,
      price: price.value,
      image: image.value
    })
    message.value = 'Товар оновлено!'
    setTimeout(() => {
      location.reload()
    }, 1000)
  } catch (err) {
    message.value = 'Помилка при оновленні товару.'
  }
}
</script>

<template>
  <div class="form-edit">
    <button @click="toggleForm">
      {{ showForm ? 'Сховати форму' : 'Редагувати товар' }}
    </button>

    <form v-if="showForm" @submit.prevent="updateItem">
      <label>Назва:</label>
      <input v-model="title" type="text" />

      <label>Ціна:</label>
      <input v-model="price" type="number" />

      <label>Фото (назва файлу):</label>
      <input v-model="image" type="text" />

      <button type="submit">Оновити</button>
    </form>

    <p v-if="message">{{ message }}</p>
  </div>
</template>


<style scoped>
.form-edit {
  max-width: 400px;
  margin: 30px auto;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}

.form-edit button {
  display: inline-block;
  padding: 10px 16px;
  font-weight: 600;
  color: white;
  background: #3b82f6;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  margin-bottom: 12px;
}

.form-edit button:hover {
  background: #2563eb;
}

.form-edit form {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.form-edit input {
  padding: 8px 12px;
  border: 1px solid #ccc;
  border-radius: 6px;
  font-size: 14px;
}

.form-edit label {
  font-weight: 500;
  margin-bottom: 4px;
}

.form-edit p {
  margin-top: 10px;
  font-size: 14px;
  color: #16a34a;
  text-align: center;
}
</style>
