<template>
  <v-col cols="12" align="center">
    <v-img
      :src="result.imageURL"
      :alt="result.name"
      height="260"
      max-width="330"
      contain
    />

    <p class="result">{{ result.name }}</p>
  </v-col>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  props: {
    result: {
      type: Object,
      default: () => {},
    },
  },
})
</script>

<style lang="scss" scoped>
.result {
  height: 86px;
  font-size: 1.8rem;
}
</style>
