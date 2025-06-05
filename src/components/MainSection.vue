<template>
  <div class="wrapper">
    <img src="/img/Image.svg" alt="Main Image" class="main-img">
    <div>
      <img src="/img/star.svg" alt="Star Icon" class="star-icon">
      <h1>Shop</h1>
      <h2>online</h2>
      <h3>now</h3>
      <p>
        Lorem psum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
      </p>
      <button><RouterLink to="/items" active-class="active" @click="toggleMenu">Переглянути товари</RouterLink></button>
    </div>
  </div>
</template>

<script>
export default {}
</script>

<style scoped>
.wrapper {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 150px auto;
  max-width: 2000px;
  padding: 0 20px;
  flex-wrap: wrap;
}

.main-img {
  max-width: 100%;
  height: auto;
}

/* Контейнер з текстом */
.wrapper > div {
  margin-left: 50px;
  color: #373845;
  flex: 1;
}

.wrapper > div h1 {
  font-weight: 400;
  font-size: 90px;
}

.wrapper > div h2,
.wrapper > div h3 {
  font-weight: 400;
  font-size: 70px;
}

.wrapper > div h3 {
  color: #5CDDBC;
}

.wrapper > div h1,
.wrapper > div h2,
.wrapper > div h3 {
  line-height: 1.1;
}

.wrapper > div p {
  font-size: 18px;
  max-width: 500px;
  margin: 20px 0;
}

.wrapper > div button {
  font-weight: 600;
  font-size: 18px;
  color: #2F323A;
  background: #5CDDBC;
  border-radius: 25px;
  border: 0;
  padding: 15px 25px;
  cursor: pointer;
  transition: transform 0.2s ease;
}

.wrapper > div button:hover {
  transform: translateY(-5px);
}

/* Адаптивність для мобільних */
@media (max-width: 768px) {
  .wrapper {
    flex-direction: column;
    text-align: center;
    margin: 50px 0;
  }

  .wrapper > div {
    margin-left: 0;
    margin-top: 30px;
  }

  .wrapper > div h1 {
    font-size: 50px;
  }

  .wrapper > div h2,
  .wrapper > div h3 {
    font-size: 40px;
  }

  .wrapper > div p {
    font-size: 16px;
    padding: 0 10px;
  }

  .wrapper > div button {
    font-size: 16px;
    padding: 12px 20px;
  }

  .main-img {
    width: 100%;
    height: auto;
  }

  .star-icon {
    width: 40px;
    height: auto;
  }
}
</style>
