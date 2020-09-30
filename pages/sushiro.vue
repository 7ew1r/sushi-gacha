<template>
  <v-container>
    <h1>スシローガチャ</h1>

    <v-row>
      <v-col cols="12" align="center">
        <v-img
          :src="result.imageURL"
          :alt="result.name"
          height="260"
          width="330"
        />

        <p class="result">{{ result.name }}</p>
        <div class="my-2" align="center">
          <v-btn
            x-large
            color="primary"
            :disabled="filteredMenuList().length < 1"
            @click="pressButton"
            >ガチャを回す</v-btn
          >
        </div>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12">
        <v-expansion-panels>
          <v-expansion-panel>
            <v-expansion-panel-header>
              <h2>
                絞り込み
                <span class="menu-count"
                  >{{ filteredMenuList().length }} 件</span
                >
              </h2>
            </v-expansion-panel-header>

            <v-expansion-panel-content>
              <v-col cols="12">
                <h3>カテゴリー</h3>
              </v-col>

              <v-treeview
                v-model="checkedCategories"
                :items="createCategoryItems(categories)"
                :selectable="true"
              />

              <!-- <v-col cols="12">
                <h3>提供エリア</h3>
              </v-col>

              <v-treeview
                v-model="checkedAreas"
                :items="createAreaItems(areas)"
                :selectable="true"
              /> -->

              <v-col cols="12">
                <h3>価格</h3>
              </v-col>

              <v-treeview
                v-model="checkedPrices"
                :items="createPricesItems(prices)"
                :selectable="true"
              />
            </v-expansion-panel-content>
          </v-expansion-panel>
        </v-expansion-panels>
      </v-col>
    </v-row>

    <Summary v-if="histories.length > 0" :histories="histories" />

    <History v-if="histories.length > 0" :histories="histories" />
  </v-container>
</template>

<script>
import menu from '../assets/sushiro.json'
import Summary from '../components/Summary.vue'
import History from '../components/History.vue'

export default {
  components: {
    Summary,
    History,
  },
  data() {
    return {
      count: 0,
      result: { name: '', imageURL: '' },
      menu,
      categories: [],
      checkedCategories: [1, 2, 3, 4, 5, 6],
      // areas: [],
      // checkedAreas: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11],
      prices: ['100円', '101 〜 200円', '201円以上'],
      checkedPrices: [1, 2, 3],
      histories: [],
    }
  },
  created() {
    this.categories = this.getCategories()
    // this.areas = this.getAreas()
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
    // getAreas() {
    //   return Array.from(new Set(this.menu.map((item) => item.area)))
    // },
    // createAreaItems(areas) {
    //   const children = areas.map(function (item, index) {
    //     return { id: index + 1, name: item }
    //   })
    //   return [{ id: 0, name: '全提供エリア', children }]
    // },
    getPrices() {
      return Array.from(new Set(this.menu.map((item) => item.price)))
    },
    createPricesItems(prices) {
      const children = prices.map(function (item, index) {
        return { id: index + 1, name: item }
      })
      return [{ id: 0, name: '全価格', children }]
    },
    filteredMenuList() {
      const selectedCategories = this.categories.filter((_, index) =>
        this.checkedCategories.includes(index + 1)
      )
      // const selectedAreas = this.areas.filter((_, index) =>
      //   this.checkedAreas.includes(index + 1)
      // )
      const selectedPrices = this.prices.filter((_, index) =>
        this.checkedPrices.includes(index + 1)
      )

      function filterByPrice(priceStr) {
        const toInt = (str) => {
          const parsed = parseInt(str, 10)
          if (isNaN(parsed)) {
            return 0
          }
          return parsed
        }
        const price = toInt(priceStr)

        for (const slectedPriceLabel of selectedPrices) {
          switch (slectedPriceLabel) {
            case '100円':
              if (price === 100) {
                return true
              }
              break
            case '101 〜 200円':
              if (price >= 101 && price <= 200) {
                return true
              }
              break
            case '201円以上':
              if (price >= 201) {
                return true
              }
              break
          }
        }
        return false
      }

      return menu
        .filter((item) => selectedCategories.includes(item.category))
        .filter((item) => filterByPrice(item.price))
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
    sumPrice() {
      const toInt = (str) => {
        const parsed = parseInt(str, 10)
        if (isNaN(parsed)) {
          return 0
        }
        return parsed
      }

      return this.histories.reduce((p, x) => p + toInt(x.menu.price), 0)
    },
    sumCalorie() {
      const toInt = (str) => {
        const parsed = parseInt(str, 10)
        if (isNaN(parsed)) {
          return 0
        }
        return parsed
      }

      return this.histories.reduce((p, x) => p + toInt(x.menu.calorie), 0)
    },
  },
}
</script>

<style scoped>
.menu-count {
  font-size: 1rem;
  font-weight: normal;
}

.result {
  height: 86px;
  font-size: 1.8rem;
}
</style>
