<script>
    export default {
        name: "Banner"
    }
</script>

<template>
    <div id="main-banner">
        <h1>Make your Burger</h1>
    </div>
</template>

<style scoped>
    #main-banner {
        background-image: url(/img/burger.jpg);
        background-size: cover;
        background-repeat: no-repeat;
        height: 500px;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }

    #main-banner h1 {
        color: #FFF;
        text-align: center;
        font-size: 60px;
        background-color: #222;
        padding: 20px;
    }
</style>