<template>
  <v-list-item three-line>
    <v-list-item-content>
      <v-list-item-title
        ><span>{{ item.API }}</span></v-list-item-title
      >
      <v-list-item-subtitle>
        {{ item.Description }}
      </v-list-item-subtitle>
      <v-list-item-subtitle>
        <span>Auth: </span>{{ item.Auth }}
      </v-list-item-subtitle>
      <v-list-item-subtitle>
        <span>HTTPS:</span> {{ item.HTTPS }}
      </v-list-item-subtitle>
      <v-list-item-subtitle>
        <span>Cors:</span> {{ item.Cors }}
      </v-list-item-subtitle>
      <v-list-item-subtitle>
        <span>Category: </span>{{ item.Category }}
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

<style scoped>
span {
  font-weight: 800;
}
</style>
