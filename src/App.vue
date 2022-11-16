<template>
  <div class="app-container">
    <SearchInput @on-search="handleSearchInput" />
    <ListContainer :items="paginate(items)" />
    <PaginationContainer
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

export default {
  name: 'App',
  components: {
    SearchInput,
    ListContainer,
    PaginationContainer,
  },
  data() {
    return {
      pageNumber: 1,
      items: [],
      pageSize: 5,
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
    const data = await this.getEntriesData();
    this.items = data.entries;
  },
  methods: {
    async getEntriesData() {
      try {
        const res = await axios.get('https://api.publicapis.org/entries');
        return res.data;
      } catch (error) {
        console.log(error.response);
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
      this.items = searchItems;
    },
  },
};
</script>

<style scoped>
.app-container {
  margin: 20px auto;
  /* display: flex;
  align-items: center;
  justify-content: center; */
}
</style>
