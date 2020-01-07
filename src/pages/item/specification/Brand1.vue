<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="brands"
      :options.sync="options"
      :server-items-length="totalDesserts"
      :loading="loading"
      class="elevation-1"
    ></v-data-table>
  </div>
</template>

<script>
    export default {
        name: "MyBrand",
      data () {
        return {
          totalDesserts: 0,
          desserts: [],
          loading: true,
          options: {},
          headers: [
            {text: '品牌id', align: 'center', value: 'id', sortable: true},
            {text: '品牌名称', align: 'center', value: 'name', sortable: false},
            {text: '品牌LOGO', align: 'center', value: 'image', sortable: false},
            {text: '品牌首字母', align: 'center', value: 'letter', sortable: true},
            {text: '操作', align: 'center', sortable: true}
          ],
        }
      },
      watch: {
        options: {
          handler () {
            this.getDataFromApi()
              .then(data => {
                this.desserts = data.items
                this.totalDesserts = data.total
              })
          },
          deep: true,
        },
      },
      mounted () {
        this.getDataFromApi()
          .then(data => {
            this.desserts = data.items
            this.totalDesserts = data.total
          })
      },
      methods: {
        getDataFromApi () {
          this.loading = true
          return new Promise((resolve, reject) => {
            const { sortBy, descending, page, itemsPerPage } = this.options

            let items = this.getDesserts()
            const total = items.length

            if (this.options.sortBy) {
              items = items.sort((a, b) => {
                const sortA = a[sortBy]
                const sortB = b[sortBy]

                if (descending) {
                  if (sortA < sortB) return 1
                  if (sortA > sortB) return -1
                  return 0
                } else {
                  if (sortA < sortB) return -1
                  if (sortA > sortB) return 1
                  return 0
                }
              })
            }

            if (itemsPerPage > 0) {
              items = items.slice((page - 1) * itemsPerPage, page * itemsPerPage)
            }

            setTimeout(() => {
              this.loading = false
              resolve({
                items,
                total,
              })
            }, 1000)
          })
        },
      },
    }
</script>

<style scoped>

</style>
