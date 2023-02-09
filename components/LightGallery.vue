<template>
    <div>
        <div class="image-container">
            <div class="image-with-flex">
                <div class="one-image-container" v-for="(product, index1) in images" :key="index1">
                    <img @click.prevent="show(index1)" class="image" :src="product.image" alt="" />
                </div>
            </div>
        </div>

        <div v-if="visible" class="light-gallery-container">
            <div class="light-gallery">
                <div class="flex-container">
                    <div class="action-icon-container" @click.stop="">
                        <p>{{ index + 1 }} / {{ images.length }}</p>
                        <p>
                            <img class="action-icon" @click="previous" src="@/assets/previousIcon.png" alt="" />
                        </p>
                    </div>
                    <div class="contain">
                        <div class="image-container-light-gallery" @click.stop="">
                            <img class="img" :src="images[index].image" alt="" />
                        </div>
                    </div>
                    <div class="action-icon-container">
                        <p>
                            <img @click="hide" class="cross-icon" src="@/assets/crossIcon.png" alt="" />
                        </p>
                        <p>
                            <img class="action-icon" @click.stop="next" src="@/assets/nextIcon.png" alt="" />
                        </p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: ['images'],
    data() {
        return {
            visible: false,
            index: 0,
        }
    },
    methods: {
        // Function is used for show light gallery
        show(currIndex) {
            this.index = currIndex;
            this.visible = true;
        },

        // Function is used for hide light gallery
        hide() {
            this.visible = false
            this.index = 0
        },

        // Function is uesd to show next image when click on next arrow icon
        next() {
            this.index = this.index + 1
            if (this.index >= this.images.length) this.index = 0
        },

        // Function is uesd to show previous image when click on previous arrow icon
        previous() {
            this.index = this.index - 1
            if (this.index < 0) this.index = this.images.length - 1
        },
    },
}
</script>

<style scoped>
.light-gallery-container {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    width: 100%;
    height: 100%;
    max-width: 100%;
    z-index: 10000 !important;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: rgb(0, 0, 0, 0.5);
}

.flex-container {
    display: flex;
    justify-content: space-between;
}

.light-gallery {
    width: 70%;
    box-sizing: border-box;
    border-radius: 10px;
    position: fixed;
    z-index: 333;
    background-color: #fff;
}

.image-container-light-gallery {
    width: 100%;
    height: auto;
}

.contain {
    width: 40%;
    height: auto;
}

.img {
    width: 100%;
    height: 100%;
}

.action-icon {
    height: 20px;
    width: 20px;
    position: fixed;
    top: 45%;
    cursor: pointer;
}

.cross-icon {
    width: 13px;
    height: 13px;
    cursor: pointer;
}

.action-icon-container {
    padding: 5px 20px;
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
    overflow: hidden;
}

.one-image-container:hover img.image {
    transform: scale(1.09);
    cursor: zoom-in;
    transition: 0.8s all ease-in-out;
}

.image {
    width: 100%;
}

@media screen and (max-width: 768px) {

    .light-gallery {
        width: 90%;
    }

    .image-container-light-gallery {
        width: 100%;
        height: 100%;
    }

    .contain {
        width: 50%;
    }

}
</style>
