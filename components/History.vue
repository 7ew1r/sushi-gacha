<template>
  <v-row>
    <v-col cols="12">
      <v-card>
        <v-card-title><h3>履歴</h3></v-card-title>

        <v-card-text>
          <v-simple-table>
            <template v-slot:default>
              <thead>
                <tr>
                  <th class="text-left">名前</th>
                  <th class="text-left">価格</th>
                  <th class="text-left">カロリー</th>
                  <th></th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="history in histories" :key="history.no">
                  <td class="text-left">{{ history.menu.name }}</td>
                  <td :width="120" class="text-right">
                    {{ history.menu.price }} 円(税抜)
                  </td>
                  <td :width="120" class="text-right">
                    {{ history.menu.calorie }} kcal
                  </td>
                  <td :width="120">
                    <v-chip @click="deleteHistory(history.no)"> 削除 </v-chip>
                  </td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
</template>

<script lang="ts">
import Vue, { PropOptions } from 'vue'

interface Menu {
  category: string
  name: string
  price: string
  calorie: string
  imageURL: string
}

interface History {
  no: number
  menu: Menu
}

export default Vue.extend({
  props: {
    histories: { type: Array, default: () => {} } as PropOptions<History[]>,
  },
  methods: {
    deleteHistory(no: number) {
      this.histories.splice(
        this.histories.findIndex((v) => v.no === no),
        1
      )
    },
  },
})
</script>
