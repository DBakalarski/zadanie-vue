<template>
  <h1>App</h1>
  <ListContainerVue :items="paginate(items, 50, 1)" />
</template>

<script>
import axios from 'axios';
import ListContainerVue from './components/ListContainer.vue';

export default {
  name: 'App',
  components: {
    ListContainerVue,
  },
  data() {
    return {
      items: [],
    };
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
    paginate(data, pageSize, pageNumber) {
      return data.slice((pageNumber - 1) * pageSize, pageNumber * pageSize);
    },
  },
};
</script>

<style></style>
