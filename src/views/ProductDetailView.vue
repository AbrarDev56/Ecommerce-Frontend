<script setup>
    import { useRoute } from 'vue-router';
    import { onMounted, ref } from 'vue'
    import axios from 'axios'

    import { Swiper, SwiperSlide } from 'swiper/vue';

    // Import Swiper styles
    import 'swiper/css';

    // import "swiper/css/free-mode"
    // import "swiper/css/navigation"
    // import "swiper/css/thumbs"
    // import './style.css';
    import { FreeMode, Thumbs } from 'swiper';
    let thumbsSwiper = null;

    const setThumbsSwiper = (swiper) => {
        thumbsSwiper = swiper;
    }

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
    <!-- Medium and Bigger Screen Product Detail -->
    <div class="container-fluid mt-4">
        <div class="row">
            <div class="col">
                <div >
                    <swiper @swiper="setThumbsSwiper" :direction="'vertical'"  :slidesPerView="3" :freeMode="true" :modules=[FreeMode,Thumbs] class="Swiper2">
                        <swiper-slide v-for="picture in pictures"><img :src="url + picture.attributes.url" class="img-fluid mb-2" style="width: 100px;"></swiper-slide>
                    </swiper>
                </div>
            </div>
            <div class="col">
                <div style="max-width: 500px;">
                    <swiper :loop="true" :spaceBetween="1" :thumbs="{ swiper: thumbsSwiper }" :modules=[FreeMode,Thumbs] class="Swiper">
                        <swiper-slide v-for="picture in pictures"><img :src="url + picture.attributes.url" class="img-fluid mb-2" style="width: 500px;"></swiper-slide>
                    </swiper>
                </div>
            </div>
            <div class="col">
                <h1 class="mt-2">{{ name }}</h1>
                <div>${{ price }}</div>
            </div>
        </div>
    </div>

    <!-- <div id="carouselExampleIndicators" class="carousel slide" data-bs-ride="true">
        <div class="carousel-indicators">
            <div v-for="(picture, index) in pictures">
            <button type="button" data-bs-target="#carouselExampleIndicators" data-bs-slide-to="0" class="active" aria-current="true" aria-label="Slide 1"></button>
            </div>
            <button type="button" data-bs-target="#carouselExampleIndicators" data-bs-slide-to="1" aria-label="Slide 2"></button>
            <button type="button" data-bs-target="#carouselExampleIndicators" data-bs-slide-to="2" aria-label="Slide 3"></button>
        </div>
        <div class="carousel-inner">
            <div v-for="(picture, index) in pictures">
                index:{{index}}
                <div :class="['carousel-item', index === 0 , 'active']">
                    <img :src="url + picture.attributes.url" class="d-block w-100" alt="...">
                </div>
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
    </div> -->


    <!-- Small Screen Product Detail -->
    <!-- <div class="container d-none d-lg-block mt-4">
        <div class="row">
            <div class="col col-sm-2 col-lg-1">
                <div v-for="picture in pictures">
                    <img :src="url + picture.attributes.url" class="img-fluid mb-2">
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
    </div> -->
</template>
