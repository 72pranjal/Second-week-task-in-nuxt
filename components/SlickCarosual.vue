<template>
    <div class="slick-carosual-container">
        <VueSlickCarousel v-if="PdpProduct.name" v-bind="slickOptions">
            <div class="product-conatiner-slick" v-for="(product, index) in PdpProduct.similar_products" :key="index">
                   <div>
                    <NuxtLink :to="'/products/' + product.url_key">
                    <img class="image-in-slick" :src="product.image" alt="" />
                    </NuxtLink> 
                    <p>{{ product.name }}</p>
                    <p>Rs. {{ product.price }}</p>
                   </div>
            </div>
        </VueSlickCarousel>
    </div>
</template>

<script>
import VueSlickCarousel from 'vue-slick-carousel'
import 'vue-slick-carousel/dist/vue-slick-carousel.css'
import 'vue-slick-carousel/dist/vue-slick-carousel-theme.css'

export default {
    components: {
        VueSlickCarousel,
    },
    props: ['PdpProduct'],
    data() {
        return {
            slickOptions: {
                slidesToShow: 4,
                arrows: true,
                swipeToSlide: true,
                autoplaySpeed: 2000,
                pauseOnHover: false,
                responsive: [
                    {
                        breakpoint: 875,
                        settings: {
                            slidesToShow: 2,
                            slidesToScroll: 1,
                            infinite: true,
                            arrows: true
                        },
                    },
                ],
            },
        }
    },
}
</script>

<style>
.slick-carosual-container {
    width: 100%;
}

.product-conatiner-slick {
    padding: 20px;
   width: 100%;
}

.image-in-slick {
    width: 90%;
    height: auto;
    cursor: pointer;
}

.slick-next, .slick-prev {
    display: block;
    background-color: red !important;
    padding: 20px;
    text-align: center;
    border-radius: 50px;
    color: black !important;
}

@media screen and (max-width: 875px) {
    .slick-next, .slick-prev {
        display: none !important;
    }
}


</style>
