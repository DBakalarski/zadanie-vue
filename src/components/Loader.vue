<template>
  <div class="text-center">
    <v-progress-circular
      indeterminate
      color="primary"
      class="loader"
      :size="70"
    ></v-progress-circular>
  </div>
</template>

<script>
export default {
  name: 'LoaderContainer',
};
</script>
