<template>
    <div>
        <div v-if="isShowSortingList && !isShowMobileFiler" class="sorting-option-container">
            <div @click="isShowSortingList = !isShowSortingList" class="transparent-container">
                <div @click.stop="" class="sorting-option">
                <ul>
                    <li class="top-list-heading">Sort: By</li>
                    <li 
                    v-for="(sortoption, index) in sortingOptions"
                    @click="getOptionValue(sortoption.code)"
                     :key="index"
                     class="sorting-options"
                     >
                        {{ sortoption.label }}
                    </li>
                </ul>
            </div>
            </div>
        </div>
        <div class="sort-filter-container">
            <div class="sort-button">
                <button v-if="!isShowMobileFiler" @click="isShowSortingList = !isShowSortingList" class="action-button" type="button">
                    <img class="imp-icon" src="@/assets/sortingIcon.png" alt="">
                    <p>SORT: BY</p>
                </button>
                <button @click="hideMobileFilter" v-else type="button"  class="close-button">
                  CLOSE
                </button>
            </div>
            <div class="filter-button">
                <button v-if="!isShowMobileFiler" @click="showMobileFIlter" class="action-button" type="button">
                    <p><img class="imp-icon"  src="@/assets/filterIcon.avif" alt=""></p>
                    <p>FILTER</p>
                </button>

                <button v-else @click="hideMobileFilter" class="apply-button" type="button">Apply</button>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    name: "SortAndFilterBar",
    props: ["sortingOptions", "appliedFilterLength"],
    data() {
        return {
            isShowSortingList: false,
            isShowMobileFiler: false,
            isHideMobileFIlter: false,
        };
    },
    methods: {
        // Function is used to show filters in mobile view
        showMobileFIlter() {
            this.isShowMobileFiler = true;
            this.isShowSortingList = false
            this.$emit("showMobileFIlter", this.isShowMobileFiler)
        },
        // Function is used to hide filters in mobile view
        hideMobileFilter() {
           this.isShowMobileFiler = false
            this.$emit("showMobileFIlter", this.isShowMobileFiler)
        },
        // Function is used to get the value of selected sorting options
        getOptionValue(option) {
            this.$emit("getSelectedSort", option)
            this.isShowSortingList = false
        }
    }
};
</script>

<style scoped>
.sort-filter-container {
    width: 100%;
    max-width: 100%;
    background-color: #fff;
    position: fixed;
    z-index: 10000;
    bottom: 0px;
    left: 0px;
    right: 0px;
    display: flex;
    border-top: 1px solid #ccc;
    justify-content: space-between;
}

.sort-button {
    width: 49%;
}
.imp-icon {
    width: 16px;
    height: 16px;
}

.filter-button {
    width: 49%;
}
li {
    list-style: none;
    text-align: center;
    margin: 0px;
    padding: 14px 0px;
    cursor: pointer;
    color: #000;
    border-bottom: 1px solid #ccc;
}
.sorting-options {
    font-size: 16px;
    font-weight: 600;
}

ul {
    padding: 0px;
    margin: 0px;
}

.action-button {
    display: flex;
    justify-content: center;
    column-gap: 10%;
    align-items: center;
    width: 100%;
    background-color: #fff;
    color: #000;
    border: none;
    font-size: 14px;
    font-weight: 600;
    cursor: pointer;
}
.top-list-heading {
    font-size: 20px;
    font-weight: 800;
    color: #000;
}
.sorting-option {
    position: fixed;
    width: 100%;
    top: auto;
    margin: auto;
    padding: 0;
    background-color: #fff;
    bottom: 50px;
}
.transparent-container {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    opacity: 1;
    width: 100%;
    height: 100vh;
    z-index: 34;
    background-color: rgb(0, 0, 0, .5);
}
.close-button {
    width: 100%;
    text-align: center;
    border: none;
    background-color: #fff;
    color: #000;
    padding: 14px 0px;
    font-size: 16px;
    font-weight: 700;
}
.apply-button {
    width: 100%;
    padding: 14px 0px;
    text-align: center;
    border: none;
    background-color: #fff;
    color: red;
    font-size: 16px;
    font-weight: 700;
}
</style>
