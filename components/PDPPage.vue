<template>
    <div class="pdp-container">
        <div class="content-container">
            <div class="parent-div">
                <div class="image-container">
                    <div class="image-with-flex">
                        <div class="one-image-container" v-for="(product, index) in PdpProduct.gallery" :key="index">
                            <img class="image" :src="product.image" alt="" />
                        </div>
                    </div>
                </div>

                <div class="property-container">
                    <div class="name-continer">
                        <span class="product-name">{{ PdpProduct.name }}</span>
                    </div>
                    <div class="name-conatiner">
                        <span class="product-price">Rs. {{ PdpProduct.price }}</span>
                        <p class="text">inclusive of all taxes</p>
                    </div>
                    <div class="action-btn-container">
                        <span>
                            <button class="addcart-btn">ADD TO CART</button>
                        </span>
                        <span>
                            <button class="whislist-btn">WISHLIST</button>
                        </span>
                    </div>
                </div>
            </div>
            <!-- Similar product section.......................... -->
            <div class=" similar-product-container">
                <div class="container-title">Similar Product</div>
                <SlickCarosual :PdpProduct="PdpProduct" />
            </div>
        </div>
    </div>
</template>

<script>
import SlickCarosual from './SlickCarosual.vue'

export default {
    data() {
        return {
            urlKey: this.$route.params.name,
            PdpProduct: {},
        };
    },
    async fetch() {
        let data = await fetch(`https://pim.wforwoman.com/pim/pimresponse.php/?service=product&store=1&url_key=${this.urlKey}`).then((res) => res.json());
        this.PdpProduct = data.result;
        console.log("adasd", data);
    },
    components: { SlickCarosual }
}
</script>

<style scoped>
.pdp-container {
    width: 100%;
    max-width: 100%;
    background-color: #fff;
}

.content-container {
    padding: 20px 15px;
}

.parent-div {
    display: flex;
    flex: 0 0 100%;
    justify-content: space-between;
}

.image-container {
    flex: 0 0 58%;
}

.property-container {
    flex: 0 0 38%;
}

.image-with-flex {
    display: flex;
    flex-wrap: wrap;
    row-gap: 10px;
    flex: 0 0 100%;
    justify-content: space-between;
}

.one-image-container {
    flex: 0 0 49%;
}

.image {
    width: 100%;
}

.product-name {
    font-size: 22px;
    font-weight: 500;
    color: #000;
}

.name-continer {
    padding-bottom: 20px;
}

.product-price {
    font-size: 30px;
    color: #000;
    font-weight: 500;
}

.text {
    color: #000;
    font-weight: 600;
}

.action-btn-container {
    display: flex;
    column-gap: 10px;
}

.addcart-btn {
    padding: 10px 70px;
    background-color: #501337;
    color: #fff;
    font-size: 16px;
    font-weight: 500;
    border-radius: 10px;
    cursor: pointer;
}

.whislist-btn {
    padding: 10px 40px;
    border: 1px solid #ccc;
    background-color: #fff;
    color: #000;
    font-weight: 500;
    font-size: 16px;
    border-radius: 10px;
    cursor: pointer;
}

.similar-product-container {
    padding: 30px 60px;
}
.container-title {
    text-align: center;
    padding: 30px 0px;
    font-size: 20px;
    color: #000;
    font-weight: 600;
}
</style>
