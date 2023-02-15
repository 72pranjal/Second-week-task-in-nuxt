<template>
    <div class="slick-carosual-container">
        <VueSlickCarousel v-if="PdpProduct.name" v-bind="slickOptions">
            <div class="product-conatiner-slick" v-for="(product, index) in PdpProduct.similar_products" 
            :key="index" 
            @mouseover="showViewDetails(product.id_product)"
            @mouseleave="hideViewDetails"
            >
                <div class="image-container-slick-carousel">
                    <NuxtLink :to="'/products/' + product.url_key">
                        <img class="image-in-slick" :src="product.image" alt="" />
                        <div v-if="currentProductId.includes(product.id_product)" class="button-conatiner-slick-carousel">
                        <button class="show-button-slick-carousel">VIEW DETAIL</button>
                    </div>
                    </NuxtLink>
                </div>
                    <p class="name">{{ product.name }}</p>
                    <p class="name">Rs. {{ product.price }}</p>
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
            currentProductId: []
        }
    },
    methods: {
        showViewDetails(id) {
            this.currentProductId.push(id)
        },
        hideViewDetails() {
            this.currentProductId.splice(0, this.currentProductId.length)
        }
    }
}
</script>

<style>
.slick-carosual-container {
    width: 100%;
}

.product-conatiner-slick {
    padding: 10px;
    width: 100%;
}

.image-in-slick {
    width: 90%;
    height: auto;
    cursor: pointer;
}

.slick-next {
    padding: 20px;
    position: absolute;
    z-index: 234234;
    top: 40%;
    right: 30px;
}
.slick-prev:before, .slick-next:before {
    font-size: 30px !important;
}

.slick-prev {
    padding: 20px;
    position: absolute;
    z-index: 234234;
    top: 40%;
    left: 0.01%;
}

.name {
    color: #0c0c0c;
    font-weight: 400;
    padding: 0;
    width: 95%;
}
.image-container-slick-carousel {
    position: relative;
    cursor: pointer;
}
.button-conatiner-slick-carousel {
  position: absolute;
  background-color: #fff;
  bottom: 3px;
  padding-top: 8px;
  width: 90.5%;
  text-align: center;
}
.show-button-slick-carousel {
  border: 1px solid #ccc;
  background-color: #fff;
  color: #000;
  font-size: 14px;
  font-weight: 500;
  font-family: "jost-medium";
  padding: 6px 60px;
  cursor: pointer;
  border-radius: 3px;
}

.show-button-slick-carousel:hover {
  border: 1px solid #000;
}

@media screen and (max-width: 875px) {

    .slick-next,
    .slick-prev {
        display: none !important;
    }
}
</style>
