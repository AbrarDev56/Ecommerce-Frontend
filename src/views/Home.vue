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
    <div v-for="product in products" v-bind:key="product.id" class="mx-3">
        <div class="card mb-3 mt-3 mx-auto" style="max-width: 1000px;">
            <div class="row g-0">
                <div class="col-md-4">
                    <!-- <img :src="url + product.attributes.thumbnail.data.attributes.url" class="img-fluid"> -->
                </div>
                <div class="col-md-8">
                    <div class="card-body">
                        <h5 class="card-title">{{ product.attributes.name }}</h5>
                        <h5 class="card-title">{{ product.attributes.price }}</h5>
                        <!-- <div v-html="product.attributes.article" class="card-text text-truncate" style="height: 75px"></div> -->
                        <RouterLink :to="`${product.id}`" class="stretched-link"></RouterLink>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>