<template>
    <div id="app">
        <div>
            <div class="container">
                <RenderProductContainer v-if="filtersData.length" :dataForFilters="filtersData"
                    :dataForProducts="productsData" :dataForSorting="sortingOptions" @sortData="getSortedData"
                    @getFilterString="getFilters" :totalProductCount="totalProductCount" :spinLoader="spinLoader" 
                   />
                <Pagination :currentPagination="currentPagination" @handleClickThenActive="getProductOfCurrentPage"
                    :totalpageNumber="totalpageNumber" />
            </div>
        </div>
    </div>
</template>

<script>

export default {
    name: 'MainContainer',
    data() {
        return {
            productsData: [],
            filtersData: [],
            sortingOptions: [],
            totalProductCount: 0,
            currentPagination: 1,
            totalpageNumber: 0,
            sortingValue: '',
            filterSting: '',
            spinLoader: false,
        }
    },
    async fetch() {
        this.spinLoader = true
        let data = await fetch(
            `https://pim.wforwoman.com/pim/pimresponse.php/?service=category&store=1&url_key=top-wear-kurtas&page=${this.currentPagination}&count=20&sort_by=${this.sortingValue}&sort_dir=desc&filter=${this.filterSting}`
        ).then((res) => res.json())
        this.productsData = data.result.products
        console.log("thisdssssds", data)
        this.filtersData = data.result.filters
        this.sortingOptions = data.result.sort
        this.totalProductCount = data.result.count
        this.getTotalPageCount()
        this.spinLoader = false
    },
    methods: {
        // Function is used to get data from api when user
        // Change current page
        // When user applied filter
        // when user applied sorting on data
        async getDataUpdateDataFromApi() {
            this.spinLoader = true
            let data = await fetch(
                `https://pim.wforwoman.com/pim/pimresponse.php/?service=category&store=1&url_key=top-wear-kurtas&page=${this.currentPagination}&count=20&sort_by=${this.sortingValue}&sort_dir=desc&filter=${this.filterSting}`
            ).then((res) => res.json())
            this.productsData = data.result.products
            this.totalProductCount = data.result.count
            this.getTotalPageCount()
            this.spinLoader = false
        },

        // Function is used to get current page number
        // and get data according to current page
        getProductOfCurrentPage(currentPage) {
            this.currentPagination = currentPage
            this.getDataUpdateDataFromApi()
        },

        // Function is used to check
        // If sorting option is selected then get data according to selected option
        // If sorting option is selected and current page is greater than 1 then send back to
        // first page number with selected sorting option
        getSortedData(sortedValue) {
            this.sortingValue = sortedValue
            if (this.currentPagination !== 1 && this.sortingValue !== '') {
                this.currentPagination = 1
            }
            this.getDataUpdateDataFromApi()
        },

        // Function is used check the applied filter array lenght is zero or not
        getFilters(applyFilters) {
            if (applyFilters.length) {
                if (applyFilters.length > 1) {
                    let str = ''
                    for (let i = 0; i < applyFilters.length; i++) {
                        if (!str) {
                            str = str + applyFilters[i]
                        } else {
                            str = str + ',' + applyFilters[i]
                        }
                    }
                    this.filterSting = str
                } else {
                    this.filterSting = applyFilters[0]
                }
                this.$router.push({
                    path: this.$route.fullPath,
                    query: { filter: this.filterSting },
                })
                if (this.currentPagination > 1) this.currentPagination = 1
                this.getDataUpdateDataFromApi()
            } else {
                this.filterSting = ''
                this.$router.push({
                    path: this.$route.fullPath,
                    query: { filter: this.filterSting },
                })
                this.getDataUpdateDataFromApi()
            }
        },

        // Function is used to calculate total number of page
        getTotalPageCount() {
            this.totalpageNumber = Math.ceil(this.totalProductCount / 20)
        },
    },
    mounted() {
        // this.getProductData()
    },
    watch: {
        filterSting() {
            this.getDataUpdateDataFromApi()
        },
    },
}
</script>

<style>
#app {
    background-color: #fff;
    width: 100%;
}

.container {
    padding: 0px 10px;
    font-family: Jost-regular;
}
</style>
