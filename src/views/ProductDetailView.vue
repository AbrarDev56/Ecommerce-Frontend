<script setup>
    import { useRoute } from 'vue-router';
    import { onMounted, ref } from 'vue'
    import axios from 'axios'

    const name = ref()
    const price = ref()
    // const author = ref()
    // const thumbnail = ref()
    const url = "http://localhost:1337"
    const route = useRoute();
    onMounted(() => {
        const product_detail = route.params.product_detail
        axios.get(`products/${product_detail}?populate=*`)
            .then(response => {
                name.value = response.data.data.attributes.name
                price.value = response.data.data.attributes.price
                // author.value = response.data.data.attributes.author.data.attributes.username
                // thumbnail.value = response.data.data.attributes.thumbnail.data.attributes.url
                console.log(response.data.data)
                // console.log(thumbnail.value)
                document.name = 'Ecommerce | ' + name.value
            })
            .catch(error => {
                console.log(error)
            })
    })
</script>

<template>
    <div class="home mx-auto px-3 mb-5" style="max-width: 1000px;">
        <h1 class="mt-2 text-center">{{ name }}</h1>
        <!-- <img :src="url + thumbnail" class="img-fluid mb-2">
        <b class="mt-2 text-capitalize">by {{ author }}</b> -->
        <div v-html="price"></div>
    </div>
</template>