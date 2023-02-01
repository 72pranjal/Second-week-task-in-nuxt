<template>
    <div class="story-container">
        <div class="bg-container">
            <img class="image" src="@/assets/Mask Group 48.png" alt="">
            <div class="title">
                <p class="title-text">Our Story</p>
            </div>
        </div>
    </div>

</template>

<script>

export default {
   
}
</script>

<style scoped>
.story-container {
    width: 100%;
    max-width: 100%;
}
.bg-container {
    width: 100%;
    max-width: 100%;
    padding-bottom: 20px;
    position: relative;
}
.title {
    position: absolute;
    left: 45%;
    top: 100px;
    z-index: 4234;
}
.title-text {
    color: #fff;
    font-size: 45px;
    font-weight: 400;
}
.image {
    width: 100%;
}
</style>