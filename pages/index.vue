<template>
  <div class="container m-auto">
    <div class="mt-8">
      <h1 class="text-4xl text-center font-bold">Products</h1>
      <select
        v-model="selected"
        @change="filterByCategory"
        class="
          w-full
          border
          rounded-2xl
          py-3
          px-4
          border-black-500
          bg-white
          text-gray-900
          mt-2
        "
      >
        <option value="">All</option>
        <option v-for="(cat, x) in categoryList" :key="x">
          {{ cat }}
        </option>
      </select>
      <div class="flex flex-wrap">
        <div
          v-for="(product, index) in filteredCategories"
          :key="index"
          class="sm:w-2/4 w-full md:w-2/6 xl:w-1/4 flex-grow my-6 px-4"
        >
          <div class="relative">
            <img :src="product.image" />
            <div
              class="
                absolute
                bottom-0
                w-full
                text-center
                py-2
                text-white
                bg-black bg-opacity-50
              "
            >
              <p>{{ product.name }}</p>
              <span>{{ '$' + product.price }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [],
      categoryList: [],
      selected: '',
    }
  },
  mounted() {
    this.fetchProducts()
  },
  methods: {
    async fetchProducts() {
      const res = await this.$axios
        .get('https://trayvonnorthern.com/Edgewood-API/public/api/products')
        .then((response) => {
          const data = response.data.data
          this.products = data
          for (let i = 0; i < this.products.length; i++) {
            if (!this.categoryList.includes(this.products[i].catname)) {
              this.categoryList.push(this.products[i].catname)
            }
          }
        })
      return res
    },
    filterByCategory() {
      this.products.filter((product) => !product.catname.indexOf(this.selected))
      console.log(this.selected)
    },
  },
  computed: {
    filteredCategories() {
      const self = this
      if (self.selected === '') {
        return self.products
      } else {
        return self.products.filter(function (product) {
          return self.selected === product.catname
        })
      }
    },
  },
}
</script>
