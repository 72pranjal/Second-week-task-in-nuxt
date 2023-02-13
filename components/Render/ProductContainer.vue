<template>
  <div class="main-container">
    <div class="text-container">
      <p class="middle-text">Women Pashmina Shawls</p>
      <p class="count-number">{{ totalProductCount }} Items</p>
    </div>
    <!-- Contain filter hide button and sorting options......... -->
    <div class="sort-option-container">
      <!-- hide filter button........... -->
      <div class="hide-filter-container">
        <button class="show-filter-button" @click="showSideFilters">
          <p><img class="filter-icon" src="@/assets/filterIcon.avif" alt=""></p>
          <p>{{ isHideSideFilters ? 'HIDE FILTER' : 'SHOW FILTER' }}</p>
        </button>
      </div>
      <!-- applied filters chips........... -->
      <div class="applied-chip-container" v-if="appliedFiltersForChips.length">
        <div class="cross-icon-container" v-for="(appliedFilter, index) in appliedFiltersForChips" :key="index">
          <div class="filter-chip">
            <p class="filter-name">{{ appliedFilter }}</p>
            <img @click="removeAppliedFilter(index)" class="cross-icon" src="@/assets/darkCrosIcon.png" alt="" />
          </div>
        </div>
      </div>
      <!-- sorting options dropDown................. -->
      <div class="dropdown-container">
        <select v-model="selectedSortingOption" class="drop-down-box">
          <option class="drop-options" value="" disabled selected>
            Sort By
          </option>
          <option v-for="(option, index) in dataForSorting" :key="index" class="drop-options" :value="option.code">
            {{ option.label }}
          </option>
        </select>
      </div>

      <!-- mobile view botton nav bar............................. -->
      <div class="bottom-nav-bar">
        <HeaderSortAndFilterBar :sortingOptions="dataForSorting" @showMobileFIlter="showFilters"
          @getSelectedSort="getSortValueFromMobileView" :appliedFilterLength="appliedFilter.length" />
      </div>
    </div>

    <!-- Contain side filter and product.................. -->
    <div class="filter-product-container">
      <!-- side filter bar container ........... -->
      <div v-if="isHideSideFilters" class="side-filter-container">
        <div v-if="appliedFiltersForChips.length">
          <button @click="clearAllAppliedFilters" class="clear-button">
            Clear All
          </button>
        </div>
        <!-- <div :class="isSideFiltersFixed ? 'fixed-container' : 'not-fixed'"> -->
        <div :class="isSideFiltersFixed ? '' : ''">
          <div class="filter-container" v-for="(filters, index) in dataForFilters" :key="index">
            <div class="filter-head" @click="showSubFIlters(filters.filter_lable)">
              <p class="label-text" @click="showSubFIlters(filters.filter_lable)">
                {{ filters.filter_lable }}
              </p>
              <span v-if="!particularOpenSubFilter.includes(filters.filter_lable)">
                <img class="plus-icon" src="@/assets/plusIcon.png" alt="" />
              </span>
              <span v-else>
                <img class="plus-icon" src="@/assets/minusIcon.png" alt="" />
              </span>
            </div>
            <div v-if="particularOpenSubFilter.includes(filters.filter_lable)">
              <ul class="subfilters">
                  <li v-for="(subFilter, index) in filters.options" :key="index">
                  <input class="checkbox" type="checkbox" v-model="appliedFiltersForChips"
                    @click="getAppliedFilter(subFilter.code, subFilter.value)" :id="subFilter.value"
                    :value="subFilter.value" />
                  <label :for="subFilter.value" class="lable-subfilter">{{
                    subFilter.value
                  }}</label>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
      <!-- all products container........... -->
      <div :class="[ isHideSideFilters ? 'product-container' : 'product-container-with-full' ]">
        <div class="product-with-details">
          <div v-for="(products, index) in dataForProducts" :key="index" @mouseover="showDetails(products.id_product)"
            @mouseleave="hideDetails()" class="product-image-container">
            <NuxtLink :to="'/products/' + products.url_key">
              <div class="product-image">
                <VueSlickCarousel v-if="productIds.includes(products.id_product)" :autoplay="true" :arrows="false"
                  :autoplaySpeed="2000" :pauseOnHover="false" :dots="true">
                  <img class="image-kurta" v-for=" variation, index in products.gallery" :key="index"
                    :src="variation.image" alt="">
                </VueSlickCarousel>
                <img v-else class="image-kurta" :src="products.image" alt="" />
                <div v-if="productIds.includes(products.id_product)" class="button-conatiner">
                  <button class="show-button">View Details</button>
                </div>
                <div class="heart-image-container">
                  <img class="heart-image" src="@/assets/heartImage.svg" alt="" />
                </div>
              </div>
            </NuxtLink>

            <div class="title-container">
              <p class="title">
                {{
                  products.Product_Title_FH === null
                    ? products.name
                    : products.Product_Title_FH
                }}
              </p>
              <p class="price-product">Rs. {{ products.price }}</p>
            </div>
          </div>
        </div>
        <RenderLoaderForData />
      </div>
    </div>
    <!-- Side filter in mobile vue................................................. -->
    <div v-if="showingMobileFilter" class="mobile-filter-container">
      <div class="mobile-filter">
        <div class="header">
          <p class="header-text">Filter</p>
          <div v-if="appliedFiltersForChips.length">
            <button @click="clearAllAppliedFilters" class="clear-button">
              Clear All
            </button>
          </div>
        </div>
        <div class="lable-and-options-container">
          <div class="filter-lable-container-for-scroll">
            <div class="filter-lable-container" v-for="(filters, index) in dataForFilters" :key="index">
              <button class="label-button" :style="{
                backgroundColor: particularOpenSubFilter.includes(
                  filters.filter_lable
                )
                  ? '#fff'
                  : '',
              }" @click="showSubFIlters(filters.filter_lable)">
                {{ filters.filter_lable }}
              </button>
            </div>
          </div>
          <div class="options-container">
            <div v-for="(filters, index) in dataForFilters" :key="index">
              <div v-if="particularOpenSubFilter.includes(filters.filter_lable)">
                <ul class="subfilters">
                  <li v-for="(subFilter, index) in filters.options" :key="index">
                    <input class="checkbox" type="checkbox" v-model="appliedFiltersForChips"
                      @click="getAppliedFilter(subFilter.code, subFilter.value)" :id="subFilter.value"
                      :value="subFilter.value" />
                    <label :for="subFilter.value" class="lable-subfilter">{{
                      subFilter.value
                    }}</label>
                  </li>
                </ul>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import VueSlickCarousel from 'vue-slick-carousel'
import 'vue-slick-carousel/dist/vue-slick-carousel.css'
// optional style for arrows & dots
import 'vue-slick-carousel/dist/vue-slick-carousel-theme.css'
export default {
  name: "RenderProductContainer",
  components: {
    VueSlickCarousel
  },
  props: [
    "dataForFilters",
    "dataForProducts",
    "dataForSorting",
    "totalProductCount",
    "spinLoader"
  ],
  data() {
    return {
      particularOpenSubFilter: [],
      appliedFiltersForChips: [],
      isHideSideFilters: true,
      selectedSortingOption: "",
      paginationNumbers: [1, 2, 3, 4, 5, 6],
      currentPageNumber: 1,
      appliedFilter: [],
      showingMobileFilter: false,
      scrollPosition: null,
      isSideFiltersFixed: false,
      productIds: [],
      isShowPdpPage: true,
    };
  },
  methods: {
    // Function is used for open and hide sub filters in destTop view
    showSubFIlters(productId) {
      if (!this.particularOpenSubFilter.includes(productId)) {
        this.particularOpenSubFilter.shift();
        this.particularOpenSubFilter.push(productId);
      } else {
        this.particularOpenSubFilter.pop();
      }
    },

    // Function is used to remove applied filter in chips section when click
    // on cross icon
    removeAppliedFilter(index) {
      this.appliedFiltersForChips.splice(index, 1);
      this.appliedFilter.splice(index, 1);
      this.$emit("getFilterString", this.appliedFilter);
    },

    // function is use to hideside filter
    hideSideFilter() {
      this.isHideSideFilters = false;
    },
    // function is use to show side filters filter
    showSideFilters() {
      if(this.isHideSideFilters) {
        this.isHideSideFilters = false
      } else {
        this.isHideSideFilters = true
      }
    },

    // function is use to clear all applied filters
    clearAllAppliedFilters() {
      this.appliedFiltersForChips.splice(0, this.appliedFiltersForChips.length);
      this.appliedFilter.splice(0, this.appliedFilter.length);
      this.$emit("getFilterString", this.appliedFilter);
    },

    // FUnction is used to sort products by selected options from dropdown
    sortBySelectedOption() {
      this.$emit("sortData", this.selectedSortingOption);
    },

    // Function is used get applied filter and store in array then send to api
    getAppliedFilter(codeVal, Value) {
      let ispushed = true;
      let index = null;
      let codeAndValueWithHiphan = codeVal + "-" + Value;
      for (let i = 0; i <= this.appliedFilter.length; i++) {
        if (this.appliedFilter[i] === codeAndValueWithHiphan) {
          ispushed = false;
          index = i;
          break;
        }
      }
      if (ispushed) {
        this.appliedFilter.push(codeAndValueWithHiphan);
      } else {
        this.appliedFilter.splice(index, 1);
      }
      this.$emit("getFilterString", this.appliedFilter);
    },

    // Function is use to show filter in moblie view
    showFilters(boolean) {
      this.showingMobileFilter = boolean;
    },

    // Function is use to show get selected sorting option  in moblie view
    getSortValueFromMobileView(selectedVal) {
      this.selectedSortingOption = selectedVal;
    },

    // Function is used to set selected route by user  in route
    setAppliedSortInRoute() {
      if (this.selectedSortingOption !== "") {
        this.$router.push({ path: this.$route.fullPath, query: { sort: this.selectedSortingOption } })
      }
    },

    //Function is used to get the value of sort option and applied filters from query
    getValueFromRoute() {
      this.selectedSortingOption = this.$route.query.sort
      let filterStr = this.$route.query.filter;
      if (filterStr) {
        let a = filterStr.split(',');
        for (let i = 0; i < a.length; i++) {
          let b = a[i].split("-")
          this.getAppliedFilter(b[0], b[1])
          this.appliedFiltersForChips.push(b[1])
        }
      }
      if (this.selectedSortingOption !== "")
        this.sortBySelectedOption();
    },

    // Function is use to calculate scroll position
    getCurrentPosition() {
      this.scrollPosition = window.scrollY;
      if (this.scrollPosition > 100) {
        this.isSideFiltersFixed = true
      } else {
        this.isSideFiltersFixed = false
      }

      if (this.scrollPosition > 2040) {
        this.isSideFiltersFixed = false
      }
    },

    //Function is use show view details button on mouseover
    showDetails(proId) {
      this.productIds.push(proId)
    },

    //Function is use hide view details button on mouseleave
    hideDetails() {
      this.productIds.splice(0, this.productIds.length)
    },
  },

  watch: {
    selectedSortingOption() {
      this.sortBySelectedOption();
      this.setAppliedSortInRoute();
    },
  },
  mounted() {
    this.setAppliedSortInRoute();
    if (this.$route.query.sort || this.$route.query.filter) {
      this.getValueFromRoute();
    }
    window.addEventListener("scroll", this.getCurrentPosition)
  }
};
</script>

<style scoped>
/* <!-- after nav bar text................... --> */
input[type=checkbox] {
  accent-color: #303030;
}

.title-container {
  padding-top: 15px;
  text-align: left;
  font-size: 16px;
}
.title {
  color: #0C0C0C;
  opacity: 1;
  font-weight: 500;
}
.price-product {
  color: #4C0B36;
  font-weight: 400;
}

.text-container {
  text-align: center;
  padding: 3% 0%;
  box-sizing: border-box;
  width: 100%;
}

.middle-text {
  font-size: 32px;
  color: #303030;
  margin: 0px;
}

.count-number {
  margin: 0px;
  color: #303030;
  font-size: 18px;
  font-weight: 400;
}

.sort-option-container {
  width: 100%;
  height: auto;
  display: flex;
  margin: 20px 0px;
  align-items: center;
  justify-content: space-between;
}

.hide-filter-container {
  width: 20%;
  display: block;
}

.applied-chip-container {
  width: 58%;
  height: auto;
  display: flex;
  flex-wrap: wrap;
  column-gap: 10px;
  row-gap: 10px;
  display: block;
}

.filter-chip {
  display: flex;
  column-gap: 10px;
  align-items: center;
}

.dropdown-container {
  width: 18%;
  border: 1px solid #303030;
  display: block;
  margin-right: 10px;
}

ul {
  padding: 0;
  margin: 0;
}

.hide-all-filter {
  width: 44%;
  cursor: pointer;
}

.show-filter-button {
  display: flex;
  align-items: center;
  column-gap: 10px;
  height: 40px;
  padding: 5px 15px;
  font-size: 14px;
  color: #303030;
  cursor: pointer;
  font-weight: 600;
  background-color: #ffffff;
  border: 1px solid #ccc;
}
.filter-icon {
  width: 15px;
  height: 15px;
}

.cross-icon-container {
  font-size: 13px;
  padding: 3px 12px;
  height: 20px;
  margin-left: 8px;
  border-radius: 18px;
  border: 1px solid #707070;
  display: inline-block;
}

.filter-name {
  white-space: pre;
  margin: 0;
  padding: 0;
  color: #303030;
}

.cross-icon {
  width: 8px;
  height: 8px;
  cursor: pointer;
}

.drop-down-box {
  width: 100%;
  height: 35px;
  padding: 4px;
  font-size: 16px;
  font-weight: 500;
  background: #ffffff;
  border: 1px solid #ccc;
  cursor: pointer;
}

.drop-options {
  cursor: pointer;
  font-size: 16px;
}

/* filter and product place style...................... */
.mobile-filter-container {
  display: none;
}

.filter-product-container {
  width: 100%;
  display: flex;
  /* column-gap: 20px;
   */
   justify-content: space-between;
}

.filter-container {
  border-bottom: 1px solid #707070;
  /* padding: 30px; */
}

.side-filter-container {
  flex: 0 0 20%;
  width: 20%;
  height: auto;
  box-sizing: border-box;
  background-color: #ffffff;
  font-size: 16px;
  display: block;
}

.fixed-container {
  position: fixed;
  left: 10px;
  /* z-index: 345454px; */
  top: 150px;
  width: 22%;
  max-width: 22%;
  margin-left: 3px;
  height: 100vh;
  overflow: scroll;
}

.not-fixed {
  height: auto;
}

li {
  list-style: none;
  color: #303030;
  cursor: pointer;
}

.clear-button {
  padding: 5px 12px;
  font-size: 15px;
  color: #000;
  cursor: pointer;
  border-radius: 50px;
  border: 1px solid #707070;
  background-color: #fff;
}

.clear-button:hover {
  background-color: #000;
  color: #fff;
}

.subfilters {
  margin: 0px;
  padding-bottom: 20px;
  padding-left: 0;
}

.lable-subfilter {
  cursor: pointer;
  align-items: center;
  color: #303030;
  font-weight: 500;
}

.label-text {
  color: #303030;
  font-weight: 600;
}

.filter-head {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0;
  cursor: pointer;
}

.plus-icon {
  height: 20px;
  width: 20px;
  cursor: pointer;
  align-items: center;
}

.checkbox {
  height: 18px;
  width: 18px;
  margin: 8px;
  cursor: pointer;
  background-color: black;
}

/* style for product data................................ */
.product-container {
  width: 78%;
  padding-left: 0 !important;
  padding-right: 0 !important;
  box-sizing: border-box;
  position: relative;
}

.product-container-with-full {
  width: 100%;
  padding-left: 0 !important;
  padding-right: 0 !important;
  box-sizing: border-box;
  position: relative;
}

.product-image {
  position: relative;
  cursor: pointer;
}

.carosual-image {
  width: 75%;
}

.button-conatiner {
  position: absolute;
  background-color: #fff;
  bottom: 3px;
  padding-top: 14px;
  width: 100%;
  text-align: center;
}

.show-button {
  border: 1px solid #ccc;
  background-color: #fff;
  color: #000;
  font-size: 14px;
  padding: 6px 60px;
  cursor: pointer;
  border-radius: 3px;
}

.show-button:hover {
  border: 1px solid #000;
}

.product-image-container {
  flex: 0 0 24%;
  width: 24%;
  max-width: 24%;
}

.image-kurta {
  width: 100%;
}

.product-with-details {
  display: flex;
  row-gap: 20px;
  flex: 0 0 100%;
  width: 100%;
  max-width: 100%;
  column-gap: 10px;
  flex-wrap: wrap;
}

.heart-image-container {
  position: absolute;
  top: 10px;
  right: 5px;
}

.heart-image {
  width: 27px;
}

.bottom-nav-bar {
  display: none;
}

@media  screen and (max-width: 1024px){
  .product-image-container {
    flex: 0 0 32% !important;
    width: 32% !important;
    max-width: 32% !important;
  }
}
@media screen and (max-width: 768px) {
.applied-chip-container {
  display: none;
}
  .middle-text {
    font-size: 20px;
  }

  .count-number {
    font-size: 16px;
  }

  .side-filter-container {
    display: none;
  }

  .product-container {
    width: 100%;
    position: relative;
    box-sizing: border-box;
  }
  
  .product-image-container {
    width: 48% !important;
    max-width: 48% !important;
    flex: 0 0 48% !important;
  }

  .image-kurta {
    width: 100%;
  }

  .product-with-details {
    justify-content: space-between;
    display: flex;
  }

  .hide-filter-container {
    display: none;
  }

  .dropdown-container {
    display: none;
  }

  .bottom-nav-bar {
    display: block;
  }

  /* Style for filter in mobile view................................................ */
  .mobile-filter-container {
    display: block;
    width: 100%;
    position: fixed;
    top: 0px;
    left: 0px;
    right: 0px;
    z-index: 3242;
    background-color: #fff;
  }

  .header {
    flex: 0 0 100%;
    padding: 0px 20px;
    background-color: #fff;
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-bottom: 1px solid #ccc;
  }

  .header-text {
    font-size: 18px;
    font-weight: 500;
    padding: 0px 3px;
    color: #000;
    font-family: "Jost-regular";
  }

  .filter-lable-container {
    width: 85%;
  }

  .filter-lable-container-for-scroll {
    overflow: scroll;
    width: 40%;
    height: 85vh;
  }

  .options-container {
    width: 60%;
    padding: 10px 12px;
    overflow: scroll;
    height: 100vh;
  }

  .lable-and-options-container {
    flex: 0 0 100%;
    display: flex;
    height: 100vh;
  }

  .label-button {
    width: 100%;
    border: none;
    text-align: left;
    font-size: 16px;
    font-weight: 700;
    color: #000;
    padding: 14px 13px;
  }
}

</style>
