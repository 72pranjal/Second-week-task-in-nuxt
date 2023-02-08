<template>
  <div class="pagination-container">
    <div class="total-page-container">
      <span>Page {{ currentPageNumber }} of {{ totalpageNumber }} </span>
    </div>

    <div class="counting-container">
      <span class="show-in-desktop-view">
        <button
          class="next-button"
          type="button"
          :style="{ opacity: currentPageNumber > 1 ? '' : '0.25' }"
          @click="goToPreviousPage"
        >
          Previous
        </button>
      </span>
      <span class="show-in-mobile-view">
        <img
          @click="goToPreviousPage"
          class="goto-icon"
          src="@/assets/leftarrowIcon.png"
          alt=""
          :style="{ opacity: currentPageNumber > 1 ? '' : '0.25' }"
        />
      </span>
      <button
        v-for="(pageCount, index) in paginationNumbers"
        :key="index"
        class="goto-clickable-page"
        :style="{
          backgroundColor:
            currentPageNumber === pageCount ? '#0C0C0C' : '#FFFFFF',
          color: currentPageNumber === pageCount ? 'white' : '#303030',
        }"
        type="button"
        @click="handleClickThenActive(pageCount)"
      >
        {{ pageCount }}
      </button>
      <span class="show-in-desktop-view">
        <button
          class="next-button"
          type="button"
          :style="{
            opacity: currentPageNumber < this.totalpageNumber ? '' : '0.25',
          }"
          @click="gotoNextPage"
        >
          Next
        </button>
      </span>
      <span class="show-in-mobile-view">
        <img
          class="goto-icon"
          @click="gotoNextPage"
          src="@/assets/rightArrowIcon.png"
          alt=""
          :style="{
            opacity: currentPageNumber < this.totalpageNumber ? '' : '0.25',
          }"
        />
      </span>
    </div>

    <div class="total-page-container-2">
      <span class="hidden-text">Total Page = {{ totalpageNumber }} </span>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Pagination',
  props: ['currentPagination', 'totalpageNumber'],
  data() {
    return {
      currentPageNumber: 1,
      paginationNumbers: [1, 2, 3, 4, 5, 6],
    }
  },
  methods: {
    // Function is used to goto page with decrement of one in
    // current page using previous button
    goToPreviousPage() {
      if (this.currentPageNumber > 1) {
        this.currentPageNumber -= 1
        this.$emit('handleClickThenActive', this.currentPageNumber)
      }
      if (this.currentPageNumber < this.paginationNumbers[0]) {
        this.numberWhenClickPreButton()
      }
      return
    },

    // Manage pagination number when click on the previous button
    numberWhenClickPreButton() {
      this.paginationNumbers.splice(0, this.paginationNumbers.length)
      for (
        let i = this.currentPageNumber - 5;
        i <= this.currentPageNumber;
        i++
      ) {
        this.paginationNumbers.push(i)
      }
    },

    // Function is used for pagination active button
    handleClickThenActive(pageNo) {
      if (!(this.currentPageNumber === pageNo)) {
        this.currentPageNumber = pageNo
        this.$emit('handleClickThenActive', this.currentPageNumber)
      } else {
        this.currentPageNumber = 1
      }
    },

    // Function is used to goto page with increment of one in
    // current page using next button
    gotoNextPage() {
      if (this.currentPageNumber < this.totalpageNumber) {
        this.currentPageNumber += 1
        this.$emit('handleClickThenActive', this.currentPageNumber)
      }
      if (
        this.currentPageNumber >
        this.paginationNumbers[this.paginationNumbers.length - 1]
      ) {
        this.numberWhenClickNextButton()
      }
      return
    },

    // Manage pagination number when click on the next button
    numberWhenClickNextButton() {
      this.paginationNumbers.splice(0, this.paginationNumbers.length)
      for (
        let i = this.currentPageNumber;
        i <= this.currentPageNumber + 5;
        i++
      ) {
        this.paginationNumbers.push(i)
        if (i >= this.totalpageNumber) break
      }
    },

      // Function is used to set current pageNo in route
      setCurrentPageNoInRoute() {
      this.$router.push({ path: this.$route.fullPath, query: { pageNo: this.currentPageNumber } })
    },
  },

  watch: {
    currentPagination(newVal) {
      this.currentPageNumber = this.currentPagination
      this.setCurrentPageNoInRoute();
      if(newVal > 1) window.scrollTo(80, 80)
    },
    totalpageNumber() {
      if (this.totalpageNumber <= 1) {
        this.paginationNumbers.splice(0, this.paginationNumbers.length)
      } else {
        this.paginationNumbers = [1, 2, 3, 4, 5, 6]
      }
    },
  },

  mounted() {
    this.setCurrentPageNoInRoute();
  }
}
</script>

<style scoped>
.pagination-container {
  width: 100%;
  max-width: 100%;
  flex: 0 0 100%;
  border-top: 1px solid black;
  margin: 30px 0px;
  padding: 30px 0px;
  display: flex;
  justify-content: space-between;
}

.counting-container {
  margin-left: 10px;
  display: flex;
  column-gap: 20px;
  align-items: center;
  font-size: 16px;
  color: #4c0b36;
  text-align: center;
}

.show-in-desktop-view {
  display: block;
}

.next-button {
  padding: 10px;
  color: #4c0b36;
  font-size: 16px;
  background-color: #ffffff;
  border: none;
  cursor: pointer;
}

.next-button:hover {
  color: white;
  background-color: black;
}

.show-in-mobile-view {
  display: none;
}

.hidden-text {
  display: none;
}

.goto-clickable-page {
  padding: 10px;
  margin: 0px 6px;
  border: none;
  cursor: pointer;
  font-size: 16px;
}
.goto-clickable-page:hover {
  background-color: #000;
  color: #ffffff;
}

@media screen and (max-width: 768px) {
  .pagination-container {
    display: grid;
  }

  .total-page-container {
    width: 100%;
    padding-bottom: 15px;
    text-align: center;
  }

  .counting-container {
    width: 100%;
    column-gap: 2px;
    align-items: center;
    color: #4c0b36;
  }

  .show-in-desktop-view {
    display: none;
  }

  .show-in-mobile-view {
    display: block;
  }

  .goto-icon {
    width: 16px;
    height: 16px;
  }

  .goto-clickable-page {
    padding: 10px;
    border: none;
    cursor: pointer;
    font-size: 12px;
  }
}
</style>
