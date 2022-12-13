<script setup>
    import { useRoute } from 'vue-router';
    import { onMounted, ref } from 'vue'
    import axios from 'axios'
    import { useAuthStore } from "@/stores/auth"

    const authStore = useAuthStore();

    const profile = ref()
    const id = ref()
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
                id.value = response.data.data.id
                name.value = response.data.data.attributes.name
                price.value = response.data.data.attributes.price
                thumbnail.value = response.data.data.attributes.thumbnail.data.attributes.url
                pictures.value = response.data.data.attributes.pictures.data
                // console.log(response.data.data)
                // console.log(thumbnail.value)
                // console.log(pictures.value)
                document.name = 'Ecommerce | ' + name.value
            })
            .catch(error => {
                console.log(error)
            })

        axios.get('users/me', {
            headers: {
                'Authorization': `Bearer ${authStore.token}`
            }
        })
            .then(response => {
                profile.value = (response.data.id)
                console.log(response.data.id)
            })
            .catch(error => {
                console.log(error)
            })
    })
    //http://localhost:1337/api/users/1
    const product_id = ref('')
    function addCart(product_id) {
        axios.defaults.headers.common['Authorization'] = `Bearer ${authStore.token}`;
        axios(`users/${profile.value}`, {
            cart2: [
                {
                    product: product_id,
                    quantity: 1
                }
            ]
        })
            .then(response => {
                console.log(product_id)
                console.log(response.data.data)
            })
            .catch(error => {
                console.log(error)
            })
    }
</script>

<template>
    <!-- Medium and Bigger Screen Product Detail -->
    <div class="container mt-4">
        <div class="row row-cols-1 row-cols-md-2">
            <div class="col" style="max-width: 600px;">
                <div id="carouselExampleIndicators" class="carousel slide" data-bs-ride="false">
                    <div class="carousel-indicators" style="bottom: -40px">
                        <button v-for="(picture, index) in pictures" type="button" data-bs-target="#carouselExampleIndicators" :data-bs-slide-to="index" :class="[' ', { 'active': index === 0 }]" aria-current="true" :aria-label="'Slide ' + index">
                            <img :src="url + picture.attributes.url" class="d-block w-100" alt="...">
                        </button>
                    </div>
                    <div class="carousel-inner">
                        <div v-for="(picture, index) in pictures" :class="['carousel-item', { 'active': index === 0 }]">
                            <img :src="url + picture.attributes.url" class="d-block w-100" alt="...">
                        </div>
                    </div>
                    <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleIndicators" data-bs-slide="prev">
                        <span class="carousel-control-prev-icon" aria-hidden="true"></span>
                        <span class="visually-hidden">Previous</span>
                    </button>
                    <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleIndicators" data-bs-slide="next">
                        <span class="carousel-control-next-icon" aria-hidden="true"></span>
                        <span class="visually-hidden">Next</span>
                    </button>
                </div>
            </div>
            <div class="col">
                <h1 class="mt-2">{{ name }}</h1>
                <div>${{ price }}</div>
                <button @click="addCart(id)" class="btn btn-primary btn-lg mt-2" type="button">Add to Cart</button>
            </div>
        </div>
    </div>
</template>
