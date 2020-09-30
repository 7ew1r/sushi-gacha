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
                </tr>
              </thead>
              <tbody>
                <tr v-for="history in histories" :key="history.no">
                  <td>{{ history.menu.name }}</td>
                  <td>{{ history.menu.price }} 円(税抜)</td>
                  <td>{{ history.menu.calorie }} kcal</td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
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
}
</script>
