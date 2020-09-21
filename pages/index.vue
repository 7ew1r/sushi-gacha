<template>
  <v-container>
    <h1>くらガチャ</h1>
    <div>
      <v-row>
        <v-col cols="12" align="center">
          <div class="image">
            <img :src="result.imageURL" :alt="result.name" />
          </div>
          <p class="result">{{ result.name }}</p>
          <div class="my-2" align="center">
            <v-btn x-large color="success" dark @click="pressButton"
              >ガチャを回す</v-btn
            >
          </div>
        </v-col>
        <v-col cols="12">
          <h2>カテゴリー</h2>
        </v-col>

        <v-treeview
          v-model="checkedCategories"
          :items="createCategoryItems(categories)"
          :selectable="true"
        />

        <v-col cols="12">
          <h2>提供エリア</h2>
        </v-col>

        <v-treeview
          v-model="checkedAreas"
          :items="createAreaItems(areas)"
          :selectable="true"
        />

        <v-col v-if="histories.length > 0" cols="12">
          <h2>履歴</h2>
        </v-col>

        <v-col v-if="histories.length > 0" cols="12">
          <v-simple-table>
            <template v-slot:default>
              <thead>
                <tr>
                  <th class="text-left">名前</th>
                  <th class="text-left">価格</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="history in histories" :key="history.no">
                  <td>{{ history.menu.name }}</td>
                  <td>{{ history.menu.price }}</td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
        </v-col>
      </v-row>
    </div>
  </v-container>
</template>

<script>
import menu from '../assets/output.json'

export default {
  data() {
    return {
      count: 0,
      result: { name: '', imageURL: '' },
      menu,
      categories: [],
      checkedCategories: [1, 2, 3, 4, 5, 6, 7],
      areas: [],
      checkedAreas: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11],
      histories: [],
    }
  },
  created() {
    this.categories = this.getCategories()
    this.areas = this.getAreas()
  },
  methods: {
    getCategories() {
      return Array.from(new Set(this.menu.map((item) => item.category)))
    },
    createCategoryItems(categories) {
      const children = categories.map(function (item, index) {
        return { id: index + 1, name: item }
      })
      return [{ id: 0, name: '全カテゴリー', children }]
    },
    getAreas() {
      return Array.from(new Set(this.menu.map((item) => item.area)))
    },
    createAreaItems(areas) {
      const children = areas.map(function (item, index) {
        return { id: index + 1, name: item }
      })
      return [{ id: 0, name: '全提供エリア', children }]
    },
    filteredMenuList() {
      const selectedCategories = this.categories.filter((_, index) =>
        this.checkedCategories.includes(index + 1)
      )
      const selectedAreas = this.areas.filter((_, index) =>
        this.checkedAreas.includes(index + 1)
      )
      return menu
        .filter((item) => selectedCategories.includes(item.category))
        .filter((item) => selectedAreas.includes(item.area))
    },
    pressButton() {
      if (this.filteredMenuList() < 1) {
        return
      }
      this.count++

      this.result = this.getRandomMenu(this.filteredMenuList())
      this.histories.push({ no: this.count, menu: this.result })
    },
    getRandomMenu(filtered) {
      const randomNumber = this.getRandomNumber(filtered.length)
      return filtered[randomNumber]
    },
    getRandomNumber(max) {
      return Math.floor(Math.random() * max)
    },
  },
}
</script>

<style scoped>
.image {
  height: 260px;
  width: 330px;
}

.result {
  height: 86px;
  font-size: 1.8rem;
}
</style>
