<template>
    <div class="pdp-container">
        <div class="content-container">
            <div class="breadcrumb-conatiner">

                <NuxtLink to="/" ><p class="link-for-home-page">Home</p></NuxtLink>
                <p class="slace-sym">  / </p>
                 <p class="current-location">{{ PdpProduct.name }}</p>

            </div>
            <div class="parent-div">
                <div class="image-container">
                    <LightGallery v-if="PdpProduct.name" :images="PdpProduct.gallery" />
                </div>

                <div class="property-container">
                    <div class="name-continer">
                        <span class="product-name">{{ PdpProduct.name }}</span>
                    </div>
                    <div class="name-conatiner">
                        <span class="product-price">Rs. {{ PdpProduct.price }}</span>
                        <p class="text">inclusive of all taxes</p>
                    </div>
                    <div class="size-container">
                        <p class="size-text">SELECT SIZE</p>
                        {{ removeSqureBrakets(PdpProduct.size) }}
                        <button @click="activeButton(size)" 
                        :class="[(activeSize.includes(size)) ? 'size-button-active' : 'size-button']" 
                        v-for="(size, index) in productSize" :key="index">
                            {{ size }}
                        </button>
                    </div>
                    <div class="action-btn-container">
                        <span class="addtocart-btn">
                            <button class="addcart-btn">
                                <img  class="cart-heart-image"  src="@/assets/shopping-bag.png" alt="">
                                ADD TO CART
                            </button>
                        </span>
                        <span class="whishlist-btn-container">
                            <button class="whislist-btn">
                                <img class="cart-heart-image" src="@/assets/heartIcon.png" alt="">
                                WISHLIST
                            </button>
                        </span>
                    </div>

                    <div>
                        <p class="delivery-pin">DELIVERY PIN</p>
                        <div>
                            <input class="pincode-field" type="text" placeholder="CHECK PINCODE" />
                        </div>
                    </div>
                    <div>
                        <p class="delivery-pin">{{ PdpProduct.title_other_info }}</p>
                        <p class="delivery-pin">{{ PdpProduct.description }}</p>
                    </div>
                    <div class="product-details-conatiner">
                        <!-- <div class="child-one"> -->
                        <ul class="important-data">
                            <li v-for="(info, index) in data" :key="index">
                                <span class="label">{{ info.label }}</span>
                                <span class="value">{{ info.value }}</span>
                            </li>
                        </ul>

                        <div class="show-more-btn">
                            <button v-if="showOrHideToggle" @click="addSomeData" class="more-show-data">
                                Show More Details
                            </button>
                            <button v-else @click="hideSomeData" class="more-show-data">
                                Show Less Details
                            </button>
                        </div>
                        <!-- </div> -->
                        <!-- <div class="child-one">

                        </div> -->
                    </div>
                </div>
            </div>
            <!-- Similar product section.......................... -->
            <div class="similar-product-container">
                <div class="container-title">Similar Product</div>
                <SlickCarosual v-if="PdpProduct.name" :PdpProduct="PdpProduct" />
            </div>
        </div>
    </div>
</template>

<script>
import SlickCarosual from './SlickCarosual.vue'
import LightGallery from './LightGallery.vue'

export default {
    data() {
        return {
            urlKey: this.$route.params.name,
            PdpProduct: {},
            productSize: [],
            activeSize: [],
            data: [],
            start: 0,
            end: 6,
            showOrHideToggle: true,
        }
    },
    async fetch() {
        let data = await fetch(
            `https://pim.wforwoman.com/pim/pimresponse.php/?service=product&store=1&url_key=${this.urlKey}`
        ).then((res) => res.json())
        this.PdpProduct = data.result
    },
    components: {
        SlickCarosual,
        LightGallery,
    },
    methods: {
        // Function is used to remove squre bracket from size string
        removeSqureBrakets(size) {
            if (size !== undefined) {
                let str = size.replace(/[\[\]']+/g, '')
                this.productSize = str.split(',')
            }
        },

        // Function is used to check which button is press by user
        activeButton(num) {
            if (!this.activeSize.includes(num)) {
                this.activeSize.splice(0, this.activeSize.length)
                this.activeSize.push(num)
            }
        },

        //  Function is used to get the visible attributs from the particular product
        getImportantDataForProduct() {
            if (this.PdpProduct.visible_attributes.length) {
                this.data = this.PdpProduct.visible_attributes.slice(
                    this.start,
                    this.end
                )
                if (
                    this.end >= 0 &&
                    this.end < this.PdpProduct.visible_attributes.length
                ) {
                    this.showOrHideToggle = true
                } else {
                    this.showOrHideToggle = false
                }
            }
        },

        // Add some data when user click or add more button
        addSomeData() {
            this.end = this.end + 6
            this.getImportantDataForProduct()
        },

        // Remove some data when user click or less more button
        hideSomeData() {
            this.end = 6
            this.getImportantDataForProduct()
        },
    },
    watch: {
        PdpProduct() {
            this.getImportantDataForProduct()
        },
    },
}
</script>

<style scoped>
.breadcrumb-conatiner {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    column-gap: 10px;
    padding: 8px 0px;
}
.link-for-home-page {
    color: #333;
    font-size: 16px;
}
.slace-sym {
    font-size: 20px;
    color: red;
}
.current-location {
    color: #000;
    font-size: 14px;
}

.pdp-container {
    width: 100%;
    max-width: 100%;
    background-color: #fff;
}

.content-container {
    padding: 10px 15px;
}

.parent-div {
    display: flex;
    flex: 0 0 100%;
    flex-wrap: wrap;
    justify-content: space-between;
}
.image-container {
    flex: 0 0 55%;
}

.property-container {
    flex: 0 0 42%;
}

.product-name {
    font-size: 22px;
    opacity: 60%;
    font-weight: 100;
    padding: 10px 20px 15px 0px;
}
.delivery-pin {
    margin: 15px 0px;
    font-weight: 100;
}

.name-continer {
    padding-bottom: 20px;
}

.product-price {
    font-size: 30px;
    padding-bottom: 20px;
}

.text {
    color: #000;
  font-family: "Jost-medium";
}

.action-btn-container {
    display: flex;
    column-gap: 10px;
    border-bottom: 1px solid #ccc;
    padding-bottom: 50px;
    margin-bottom: 10px;
}
.cart-heart-image {
    width: 18px;
    height: 18px;
}
.addcart-btn {
    display: flex;
    align-items: center;
    justify-content: center;
    column-gap: 15px;
    padding: 12px 55px;
    background-color: #501337;
    color: #fff;
    font-size: 16px;
    font-weight: 500;
    border-radius: 10px;
    cursor: pointer;
}

.whislist-btn {
    display: flex;
    align-items: center;
    justify-content: center;
    column-gap: 15px;
    padding: 12px 40px;
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
    padding: 10px 0px;
    font-size: 22px;
    color: #000;
}

.size-container {
   padding-top: 20px;
   padding-bottom: 30px;
}

.size-text {
    color: #000;
    font-weight: 500;
    font-size: 18px;
}

.size-button {
    margin-top: 15px;
    padding: 9px 14px;
    margin-left: 5px;
    font-size: 16px;
    border: 1px solid #ccc;
    font-weight: 500;
    cursor: pointer;
    background-color: #fff;
    color: #000;
}
.size-button-active {
    padding: 9px 14px;
    margin-left: 5px;
    font-size: 16px;
    border: 1px solid #ccc;
    font-weight: 500;
    cursor: pointer;
    background-color: #501337;
    color: #fff;
}

.size-button:hover {
    background-color: #501337;
    color: #fff;
}

.pincode-field {
    padding: 10px 12px;
    border: 1px solid #000;
    width: 50%;
}

.show-more-btn {
    width: 100%;
}

.more-show-data {
    border: none;
    color: #ee1010;
    display: block;
    cursor: pointer;
    font-size: 16px;
    padding-left: 0;
    background-color: #fff;
}

.important-data {
    display: flex;
    flex-flow: row wrap;
    margin-top: 12px;
    width: 100%;
}

.value {
    color: #757575;
    line-height: 1.5;
    width: 98%;
    font-size: 14px;
    vertical-align: top;
}

.label {
    width: 98%;
    color: #444;
    font-size: 16px;
}

ul,
li {
    padding: 0;
    margin: 0;
}

ul,
li {
    list-style: none;
    vertical-align: top;
    line-height: 15px;
    /* border-bottom: 1px solid #eaeaec; */
    margin: 0 0 8px;
    padding: 0 0 7px;
    width: 50%;
}

ul,
li,
span {
    line-height: 18px;
    vertical-align: top;
    display: inline-block;
}

@media screen and (max-width: 875px) {
    .image-container {
        flex: 0 0 100%;
    }
    .product-name {
        margin-top: 20px;
    }

    .property-container {
        flex: 0 0 100%;
    }

    .action-btn-container {
        width: 100%;
        position: fixed;
        z-index: 9999;
        left: 0;
        right: 0;
        bottom: -10px;
        padding: 10px 8px;
        background-color: #fff;
        box-shadow: 0 0 4px 0 rgb(0 0 0 / 20%);
    }

    .addtocart-btn {
        width: 58%;
    }
    .whishlist-btn-container {
        width: 40%;
    }

    .addcart-btn {
        width: 100%;
        padding: 12px;
        border-radius: 8px;
    }

    .whislist-btn {
        width: 93%;
        padding: 12px;
        border-radius: 8px;
    }
}

@media screen and (max-width: 580px) {
    .similar-product-container {
        padding: 5px 5px;
    }
}
</style>
