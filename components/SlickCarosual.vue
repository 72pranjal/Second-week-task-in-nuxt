<template>
    <div class="carosual-container">
        <VueSlickCarousel v-if="PdpProduct.name" v-bind="slickOptions">
            <div class="product-conatiner" v-for="product, index in PdpProduct.similar_products" :key="index">
                <div class="product-conatiner2">
                    <img class="image" :src="product.image" alt="">
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
    props: ["PdpProduct"],
    data() {
        return {
            slickOptions: {
                autoplay: true,
                dots: true,
                slidesToShow: 4,
                arrows: true,
                swipeToSlide: true,
                autoplaySpeed: 2000,
                pauseOnHover: false
            },
            responsive: {
                breakpoint: 768,
                settings: {
                    slidesToShow: 2,
                },
            },
        }

    }

}
</script>

<style scoped>
.carosual-container {
    width: 100%;
}

.product-conatiner {
    width: 100%;
}

.product-conatiner2 {
    width: 70%;
}

.image {
    width: 100%;
}
</style>