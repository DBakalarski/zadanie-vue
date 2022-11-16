<template>
  <div class="app-container">
    <div class="filter-container">
      <SearchInput
        :is-clear="isSearchClear"
        @clear-search="handleClearSearch"
        @on-search="handleSearchInput"
      />
      <CategoryInput
        :is-clear="isSelectClear"
        @clear-select="handleClearSelect"
        @on-select="handleSelect"
      />
    </div>

    <Loader v-if="isLoading" />
    <ListContainer v-else :items="paginate(items)" />
    <PaginationContainer
      v-if="calculatePages > 1 && !isLoading"
      :length="calculatePages"
      @change-page="handleChangePage"
    />
  </div>
</template>

<script>
import axios from 'axios';
import ListContainer from './components/ListContainer.vue';
import PaginationContainer from './components/PaginationContainer.vue';
import SearchInput from './components/SearchInput.vue';
import CategoryInput from './components/CategoryInput.vue';
import Loader from './components/Loader.vue';

export default {
  name: 'App',
  components: {
    SearchInput,
    ListContainer,
    PaginationContainer,
    CategoryInput,
    Loader,
  },
  data() {
    return {
      pageNumber: 1,
      items: [],
      pageSize: 5,
      isSelectClear: false,
      isSearchClear: false,
      isLoading: false,
    };
  },
  computed: {
    calculatePages() {
      if (this.items) {
        return Math.floor(this.items.length / this.pageSize);
      }
      return 0;
    },
  },
  async created() {
    this.getEntriesData();
  },
  methods: {
    async getEntriesData() {
      try {
        this.isLoading = true;
        const res = await axios.get('https://api.publicapis.org/entries');
        this.items = res.data.entries;
      } catch (error) {
        console.log(error.response);
      } finally {
        this.isLoading = false;
      }
    },
    paginate(data) {
      if (!data) return [];
      return data.slice(
        (this.pageNumber - 1) * this.pageSize,
        this.pageNumber * this.pageSize
      );
    },
    handleChangePage(newPageNumber) {
      this.pageNumber = newPageNumber;
    },
    handleSearchInput(searchItems) {
      this.isSelectClear = true;
      this.isSearchClear = false;
      this.pageNumber = 1;
      this.items = searchItems;
    },
    handleSelect(filteredItems) {
      this.isSearchClear = true;
      this.isSelectClear = false;
      this.pageNumber = 1;
      this.items = filteredItems;
    },
    async handleClearSelect() {
      this.getEntriesData();
    },
    async handleClearSearch() {
      this.getEntriesData();
    },
  },
};
</script>

<style scoped>
.app-container {
  min-height: 100vh;
  /* margin: 20px auto; */
  position: relative;
  padding-bottom: 80px;
}

.filter-container {
  max-width: 800px;
  margin: 0 auto;
}
@media (min-width: 650px) {
  .filter-container {
    display: flex;
    justify-content: center;
  }
}
</style>
