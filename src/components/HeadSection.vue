<template>
    <div class="wrapper">
        <div>
            <h1>Всі товари</h1>
            <h2>Найкращі товари для вас</h2>
        </div>
        <div>
            <img src="/img/Image.svg" alt="">
        </div>
    </div>
</template>

<script>
export default {}
</script>

<style scoped>
.wrapper {
    background: #5CDDBC;
    width: 104%;
    position: relative;
    right: 13%;
    display: flex;
    padding: 0 12%;
    justify-content: space-between;
    align-items: center;
    height: 240px;
    color: #216F5B;
    margin: 60px 0;
}

.wrapper > div {
    line-height: 1.1; 
}

.wrapper h1 {
    font-size: 70px;
}

.wrapper h2 {
    font-size: 30px;
    font-weight: 400;
    margin-top: 15px;
}

.wrapper img {
    height: 230px;
    position: relative;
    top: 8px;
    right: 30px;
}
@media (max-width: 600px) {
    .wrapper h1 {
        font-size: 32px;
    }

    .wrapper h2 {
        font-size: 18px;
    }

    .wrapper img {
        height: 140px;
    }
}
</style>