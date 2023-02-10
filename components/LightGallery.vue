<template>
  <div>
    <div class="image-container">
      <div class="image-with-flex" id="lightgallery">
        <div
          class="one-image-container22"
          v-for="(product, index1) in images"
          :key="index1"
          @click="gallerycontrol()"
        >
          <img
            class="image"
            :src="product.image"
            :data-src="product.image"
            alt=""
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['images'],
  methods: {
    gallerycontrol() {
      lightGallery(document.getElementById('lightgallery'), {
        download: false,
        selector: 'img',
      })
    },
  },

  mounted() {
    this.gallerycontrol()
  },
}
</script>

<style>
.image-with-flex {
  display: flex;
  flex-wrap: wrap;
  row-gap: 10px;
  flex: 0 0 100%;
  justify-content: space-between;
}

.one-image-container22 {
  flex: 0 0 49%;
  overflow: hidden;
}

.one-image-container22:hover img.image {
  transform: scale(1.09);
  cursor: zoom-in;
  transition: 0.8s all ease-in-out;
}

.image {
  width: 100%;
}

.lg-toolbar {
  background-color: rgba(0, 0, 0, 0) !important;
}

.lg-outer.lg-visible {
  background-color: #fff !important;
  width: 50% !important;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  height: 90%;
  border-radius: 12px;
}

.lg-toolbar .lg-icon:hover {
  color: #000 !important;
}

.lg-actions .lg-next:hover,
.lg-actions .lg-prev:hover {
  color: #000 !important;
}

.lg-backdrop.in {
  opacity: .6 !important;
}
</style>
