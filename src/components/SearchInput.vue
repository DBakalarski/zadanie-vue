<template>
  <v-text-field
    v-model="searchValue"
    :loading="isLoading"
    :color="inputColor"
    :clearable="true"
    class="search-input"
    label="Search"
    placeholder="Enter search phrase"
    @keyup="handleSearchValue"
    @click:clear="handleClearInput"
  ></v-text-field>
</template>

<script>
import axios from 'axios';

export default {
  name: 'SerchInput',
  props: {
    isClear: {
      required: true,
      type: Boolean,
    },
  },
  emits: ['on-search', 'clear-search'],
  data() {
    return {
      searchValue: null,
      isLoading: false,
      hasSearchResult: true,
    };
  },

  computed: {
    inputColor() {
      return this.hasSearchResult ? 'green' : 'red';
    },
  },
  watch: {
    isClear(newValue) {
      if (newValue) {
        this.searchValue = null;
      }
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

      if (!searchItems.entries) {
        this.hasSearchResult = false;
      } else this.hasSearchResult = true;
    },

    handleClearInput() {
      this.hasSearchResult = true;
      this.$emit('clear-search');
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
