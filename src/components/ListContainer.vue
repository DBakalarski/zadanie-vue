<template>
  <div class="list">List</div>
  <div v-for="item in items" :key="item.Link">
    <p>{{ item.API }}</p>
  </div>
</template>

<script>
export default {
  name: 'ListContainer',
  props: {
    items: {
      type: Array,
      required: true,
    },
  },
};
</script>
