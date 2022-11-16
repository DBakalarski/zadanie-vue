<template>
  <v-select
    v-model="selectValue"
    class="category-input"
    :items="categories"
    label="Category"
    :clearable="true"
    @click:clear="handleClearSelect"
  ></v-select>
</template>

<script>
import axios from 'axios';

export default {
  name: 'CategoryInput',
  props: {
    isClear: {
      required: true,
      type: Boolean,
    },
  },
  emits: ['on-select', 'clear-select'],
  data() {
    return {
      categories: [],
      selectValue: null,
    };
  },
  watch: {
    async selectValue(newValue, oldValue) {
      if (newValue && newValue !== oldValue) {
        const firstWord = newValue.split(' ')[0].toLowerCase();
        const searchData = await this.getSearchData(firstWord);
        this.$emit('on-select', searchData.entries);
      }
    },
    isClear(newValue) {
      if (newValue) {
        this.selectValue = null;
      }
    },
  },
  async created() {
    const data = await this.getCategories();
    this.categories = data.categories;
  },
  methods: {
    async getCategories() {
      try {
        const res = await axios.get('https://api.publicapis.org/categories');
        return res.data;
      } catch (error) {
        console.log(error.response);
      }
    },
    async getSearchData(category) {
      try {
        this.isLoading = true;
        const res = await axios.get('https://api.publicapis.org/entries', {
          params: { category },
        });
        return res.data;
      } catch (error) {
        console.log(error.response);
      } finally {
        this.isLoading = false;
      }
    },
    handleClearSelect() {
      this.$emit('clear-select');
    },
  },
};
</script>

<style>
.category-input {
  max-width: 300px;
  margin: 20px auto;
}
</style>
