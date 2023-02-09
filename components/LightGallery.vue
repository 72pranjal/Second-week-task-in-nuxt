<template>
    <div>
        <!-- <client-only> -->
        <div class="image-container">
            <div class="image-with-flex" id="lightgallery">
                <div class="one-image-container" v-for="(product, index1) in images" :key="index1"
                    @click="gallerycontrol()">
                    <img class="image" :src="product.image" :data-src="product.image" alt="" />
                </div>
            </div>
        </div>
        <!-- </client-only> -->
    </div>
</template>

<script>
export default {
    props: ['images'],
    methods: {
        gallerycontrol() {
            lightGallery(document.getElementById("lightgallery"), {
                download: false,
                selector: "img",
            });
        },
    },

    mounted() {

            this.gallerycontrol();
    }
}
</script>

<style scoped>
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

</style>
