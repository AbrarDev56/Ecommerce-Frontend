<script setup>
    import { useRoute } from 'vue-router';
    import { onMounted, ref } from 'vue'
    import axios from 'axios'

    const name = ref()
    const price = ref()
    const thumbnail = ref()
    const pictures = ref([])
    const url = "http://localhost:1337"
    const route = useRoute();
    onMounted(() => {
        const product_detail = route.params.product_detail
        axios.get(`products/${product_detail}?populate=*`)
            .then(response => {
                name.value = response.data.data.attributes.name
                price.value = response.data.data.attributes.price
                thumbnail.value = response.data.data.attributes.thumbnail.data.attributes.url
                pictures.value = response.data.data.attributes.pictures.data
                console.log(response.data.data)
                console.log(thumbnail.value)
                console.log(pictures.value)
                document.name = 'Ecommerce | ' + name.value
            })
            .catch(error => {
                console.log(error)
            })
    })
</script>

<template>
    <div class="container-fluid d-block d-lg-none mt-4">
        <div class="row">
            <div class="col col-sm-2 col-lg-1">
                <div v-for="picture in pictures">
                    <img :src="url + picture.attributes.url" class="img-fluid mb-2" style="max-width: 100px;">
                </div>
            </div>
            <div class="col">
                <img :src="url + thumbnail" class="img-fluid" style="max-width: 500px;">
            </div>
            <div class="col">
                <h1 class="mt-2">{{ name }}</h1>
                <div>${{ price }}</div>
            </div>
        </div>
    </div>

    <div class="container d-none d-lg-block mt-4">
        <div class="row">
            <div class="col col-sm-2 col-lg-1">
                <div v-for="picture in pictures">
                    <img :src="url + picture.attributes.url" class="img-fluid mb-2" style="max-width: 100px;">
                </div>
            </div>
            <div class="col">
                <img :src="url + thumbnail" class="img-fluid" style="max-width: 500px;">
            </div>
            <div class="col">
                <h1 class="mt-2">{{ name }}</h1>
                <div>${{ price }}</div>
            </div>
        </div>
    </div>
</template>