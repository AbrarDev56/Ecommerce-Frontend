<script setup>
    import { RouterLink } from 'vue-router'
    import { onMounted, ref } from 'vue'
    import axios from 'axios'

    const products = ref([])
    // const url = "http://localhost:1337"
    onMounted(() => {
        axios.get('products?populate=*')
            .then(response => {
                products.value = (response.data.data)
                console.log(response.data.data)
            })
            .catch(error => {
                console.log(error)
            })
    })
</script>

<template>
    <div class="row row-cols-1 row-cols-md-2 g-2 m-2">
        <div v-for="product in products" v-bind:key="product.id">
            <div class="col">
                <div class="card">
                    <RouterLink :to="`${product.id}`">
                        <img src="..." class="card-img-top">
                    </RouterLink>
                    <div class="card-body">
                        <RouterLink :to="`${product.id}`" class="text-dark text-decoration-none">
                            <h5 class="card-title">{{ product.attributes.name }}</h5>
                        </RouterLink>
                        <RouterLink :to="`${product.id}`" class="text-dark text-decoration-none">
                            <h5 class="card-title">{{ product.attributes.price }}</h5>
                        </RouterLink>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>