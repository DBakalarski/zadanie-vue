<template>
  <v-list-item three-line>
    <v-list-item-content>
      <v-list-item-title>{{ item.API }}</v-list-item-title>
      <v-list-item-subtitle>
        {{ item.Description }}
      </v-list-item-subtitle>
      <v-list-item-subtitle>
        <a :href="item.Link" target="_blank">{{ item.Link }}</a>
      </v-list-item-subtitle>
    </v-list-item-content>
  </v-list-item>
</template>

<script>
export default {
  name: 'ListItem',
  props: {
    item: {
      type: Object,
      required: true,
    },
  },
};
</script>
