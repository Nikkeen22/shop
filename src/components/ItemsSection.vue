<template>
  <div class="items">
    <div class="item" v-for="el in items" :key="el.slug">
      <img :src="'/img/' + el.image" :alt="el.title" />
      <h3>{{ el.title }}</h3>
      <p>{{ el.desc }}</p>
      <div class="bottom">
        <span>{{ el.price }}$</span>
        <img src="/img/add-to-basket.svg" :alt="el.title" @click="addToBasket(el)" />
      </div>

      <FormEdit
        :slug="el.slug"
        :initialTitle="el.title"
        :initialPrice="el.price"
        :initialImage="el.image"
        />
    </div>
  </div>
</template>


<script>
import axios from 'axios'
import FormEdit from './FormEdit.vue'

export default {
    props: ['addToBasket'],
    components: {
    FormEdit,
  },
    data() {
        return {
            items: []
        }
    },
    async mounted() {
        try {
            const res = await axios.get('https://my-unique-shop-backend-c2411867a870.herokuapp.com/api/items/?format=json');
            this.items = res.data
        } catch(error) {
            console.log(error)
        }

    }
}
</script>

<style scoped>
.items {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    flex-wrap: wrap;
    gap: 20px; 
}

.items .item {
    background: #F4F4F4;
    padding: 15px;
    box-sizing: border-box;
    width: calc((100% - 40px) / 3);
}

.items .item img:first-child {
    width: 100%;
    display: block;
}

.items .item h3 {
    margin: 12px 0;
    font-size: 20px;
}

.items .item p {
    margin: 10px 0;
    font-size: 15px;
    width: 90%;
}

.items .item .bottom {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 10px;
}

.items .item .bottom span {
    font-weight: 600;
    color: #216E5B;
}

.items .item .bottom img {
    cursor: pointer;
    transition: transform 500ms ease;
}

.items .item .bottom img:hover {
    transform: scale(1.3);
}




@media (max-width: 900px) {
    .items .item {
        width: calc((100% - 20px) / 2); 
    }
}

@media (max-width: 600px) {
    .items .item {
        width: 100%;
    }
    .items .item p {
        width: 100%;
    }
}

</style>