<template>
    <div class="pdp-container">
        <div class="content-container">
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
                        <button @click="activeButton(size)" class="size-button" :style="{
                            backgroundColor: activeSize.includes(size)
                                ? '#501337'
                                : '#FFFFFF',
                            color: activeSize.includes(size) ? 'white' : '#000',
                        }" v-for="(size, index) in productSize" :key="index">
                            {{ size }}
                        </button>
                    </div>
                    <div class="action-btn-container">
                        <span class="btn">
                            <button class="addcart-btn">ADD TO CART</button>
                        </span>
                        <span class="btn">
                            <button class="whislist-btn">WISHLIST</button>
                        </span>
                    </div>

                    <div>
                        <p class="product-name">DELIVERY PIN</p>
                        <div>
                            <input class="pincode-field" type="text" placeholder="CHECK PINCODE" />
                        </div>
                    </div>
                    <div>
                        <p>{{ PdpProduct.title_other_info }}</p>
                        <p>{{ PdpProduct.description }}</p>
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
    border-bottom: 1px solid #ccc;
    padding-bottom: 50px;
    margin-bottom: 10px;
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

.size-container {
    padding: 30px 0px;
}

.size-text {
    color: #000;
    font-weight: 500;
    font-size: 18px;
}

.size-button {
    padding: 9px 14px;
    margin-left: 5px;
    font-size: 16px;
    border: 1px solid #ccc;
    font-weight: 500;
    cursor: pointer;
}

.size-button:hover {
    background-color: #501337;
    color: white;
}

.pincode-field {
    padding: 10px 10px;
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
    border-bottom: 1px solid #eaeaec;
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
        border-bottom: 1px solid #ccc;
        position: fixed;
        z-index: 3434;
        left: 0;
        right: 0;
        bottom: -10px;
        width: 100%;
        padding: 15px 30px;
        background-color: #fff;
    }

    .btn {
        width: 50%;
        left: 0;
        right: 0;
    }

    .addcart-btn {
        width: 100%;
        padding: 12px;
        border-radius: 8px;
    }

    .whislist-btn {
        width: 80%;
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
