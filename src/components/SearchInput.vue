<template>
  <v-text-field
    v-model="searchValue"
    :loading="isLoading"
    :color="inputColor"
    class="search-input"
    label="Search"
    placeholder="Enter search phrase"
    @keyup="handleSearchValue"
  ></v-text-field>
</template>

<script>
import axios from 'axios';

export default {
  name: 'SerchInput',
  emits: ['on-search'],
  data() {
    return {
      searchValue: '',
      isLoading: false,
      hasSearchResult: true,
    };
  },
  computed: {
    inputColor() {
      return this.hasSearchResult ? 'green' : 'red';
    },
  },
  methods: {
    async getSearchData() {
      try {
        this.isLoading = true;
        const res = await axios.get('https://api.publicapis.org/entries', {
          params: { title: this.searchValue },
        });
        return res.data;
      } catch (error) {
        console.log(error.response);
      } finally {
        this.isLoading = false;
      }
    },
    async handleSearchValue() {
      const searchItems = await this.getSearchData();
      this.$emit('on-search', searchItems.entries);
      console.log('searchItems.entries', searchItems.entries);
      if (!searchItems.entries) {
        this.hasSearchResult = false;
      } else this.hasSearchResult = true;
    },
  },
};
</script>

<style scoped>
.search-input {
  max-width: 300px;
  margin: 20px auto;
}
</style>
