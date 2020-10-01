<template>
  <v-col cols="12" align="center">
    <div class="my-2" align="center">
      <v-btn x-large color="primary" :disabled="disable" @click="click"
        >ガチャを回す</v-btn
      >
    </div>
  </v-col>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  props: {
    disable: { type: Boolean, default: false },
    click: { type: Function, default: () => null },
  },
})
</script>
