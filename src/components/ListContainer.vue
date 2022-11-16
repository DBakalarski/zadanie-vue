<template>
  <v-card class="mx-auto" max-width="600" tile>
    <ListItem v-for="item in items" :key="item.Link" :item="item" />
  </v-card>
</template>

<script>
import ListItem from './ListItem.vue';
export default {
  name: 'ListContainer',
  components: {
    ListItem,
  },
  props: {
    items: {
      type: Array,
      required: true,
    },
  },
};
</script>
