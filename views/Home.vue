<script>
import Banner from '../components/Banner.vue'
import BurgerForm from '../components/BurgerForm.vue'
export default {
    name: "Home",
    components: {
        Banner,
        BurgerForm
    }
}
</script>

<template>
    <Banner />
    <div class="main-container">
        <h1>Create a burger: </h1>
    </div>
    <BurgerForm />
</template>