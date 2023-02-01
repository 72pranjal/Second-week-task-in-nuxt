<template>
    <div class="story-container">
        <div class="bg-container">
            <img class="image" src="@/assets/Mask Group 485.png" alt="">
            <div class="title">
                <p class="title-text">Craftmanship</p>
                <p class="text2">Unrivalled detail that elevates Pashmina to an artform</p>
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
   text-align: center;
    top: 100px;
    left: 33%;
    z-index: 4234;
}
p {
    margin: 3px;
}
.title-text {
    color: #fff;
    font-size: 35px;
    font-weight: 400;
}
.text2 {
    font-size: 20px;
    color: #fff;
}
.image {
    width: 100%;
}
</style>