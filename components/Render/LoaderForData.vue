<template>
    <div>
        <div v-if="spinLoader" class="loader-gif">
          <img class="loader" src="@/assets/loaderGIf.gif" alt="">
        </div>
    </div>
</template>

<script>

export default {
    name: "LoaderFordata",
    props: ["spinLoader"],
    mounted() {
      console.log(this.spinLoader)
    },
    watch: {
      spinLoader(newVal) {
        console.log(newVal)
      }
    }
}
</script>

<style scoped>
.loader-gif {
  position: absolute;
  left: 47%;
  right: 50%;
  top: 60%;
  bottom: 40%;
  position: fixed;
  z-index: 3456;
}
.loader {
  width: 50px;
  height: 50px;
}
</style>