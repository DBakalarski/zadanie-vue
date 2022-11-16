<template>
  <div class="text-center pagination-container">
    <v-pagination
      v-model="pageNumber"
      :length="length"
      circle
      @update:model-value="(pageNumber) => handleChangePage(pageNumber)"
    ></v-pagination>
  </div>
</template>

<script>
export default {
  name: 'PaginationContainer',
  props: {
    length: {
      type: Number,
      required: true,
    },
  },
  emits: ['change-page'],
  data() {
    return {
      pageNumber: 1,
    };
  },
  methods: {
    handleChangePage(pageNumber) {
      this.$emit('change-page', pageNumber);
    },
  },
};
</script>

<style scoped>
.pagination-container {
  max-width: 800px;
  position: absolute;
  bottom: 0;
  transform: translateX(-50%);
  left: 50%;
  padding: 5px;
  text-align: center;
}
</style>
