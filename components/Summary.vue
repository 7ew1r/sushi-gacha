<template>
  <v-row>
    <v-col cols="12">
      <v-card>
        <v-card-title><h3>要約</h3></v-card-title>

        <v-card-text class="text--primary">
          <v-row class="text-right">
            <v-col class="pr-10"
              ><span class="value">{{ histories.length }}</span>
              <span class="unit">皿 </span></v-col
            >
            <v-col class="pr-10"
              ><span class="value">{{ sumPrice() }}</span>
              <span class="unit">円(税抜)</span>
            </v-col>
            <v-col class="pr-10"
              ><span class="value">{{ sumCalorie() }}</span>
              <span class="unit">kcal</span></v-col
            >
          </v-row>
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  props: {
    histories: { type: Array, default: () => {} },
  },
  methods: {
    toInt(str) {
      const parsed = parseInt(str, 10)
      if (isNaN(parsed)) {
        return 0
      }
      return parsed
    },
    sumPrice() {
      return this.histories.reduce((p, x) => p + this.toInt(x.menu.price), 0)
    },
    sumCalorie() {
      return this.histories.reduce((p, x) => p + this.toInt(x.menu.calorie), 0)
    },
  },
}
</script>

<style scoped>
.value {
  font-size: 3rem;
}
</style>
