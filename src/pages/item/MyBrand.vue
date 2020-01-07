<template>
  <div>
    <v-layout class="px-3 pb-2">
      <v-flex xs2>
        <v-btn color="info">新增品牌</v-btn>
      </v-flex>
      <v-spacer/>
      <v-flex xs4>
        <v-text-field label="搜索" hide-details append-icon="search" v-model="key"></v-text-field>
      </v-flex>
    </v-layout>
    <v-data-table
      :headers="headers"
      :items="brands"
      :pagination.sync="pagination"
      :total-items="totalBrands"
      :loading="loading"
      class="elevation-1"
    >
      <template slot="items" slot-scope="props">
        <td class="text-xs-center">{{ props.item.id }}</td>
        <td class="text-xs-center">{{ props.item.name }}</td>
        <td class="text-xs-center"><img :src="props.item.image" alt=""></td>
        <td class="text-xs-center">{{ props.item.letter }}</td>
        <td class="text-xs-center">
          <v-icon small class="mr-2" @click="editItem(props.item)">
            edit
          </v-icon>
          <v-icon small @click="deleteItem(props.item)">
            delete
          </v-icon>
        </td>
      </template>
    </v-data-table>
  </div>
</template>

<script>
  export default {
    name: "MyBrand",
    data() {
      return {
        headers: [ // 头信息
          {text: '品牌id', align: 'center', value: 'id', sortable: true},
          {text: '品牌名称', align: 'center', value: 'name', sortable: false},
          {text: '品牌LOGO', align: 'center', value: 'image', sortable: false},
          {text: '品牌首字母', align: 'center', value: 'letter', sortable: true},
          {text: '操作', align: 'center', sortable: true}
        ],
        brands: [],
        pagination: {},
        totalBrands: 0,
        loading: false,
        key: "",
      }
    },
    created() {
      this.brands = [
        {id: 2032, name: "OPPO", image: "1.jpg", letter: "O"}
      ];
      this.totalBrands = 15;

      this.loadBrands();
    },
    watch:{
      key(){
        this.pagination.page = 1;
        this.loadBrands();
      },
      pagination:{
        deep: true,
        handler(){
          this.loadBrands();
        }
      }
    },
    methods:{
      loadBrands(){
        this.loading = true;
        this.$http.get("/item/brand/page",{
          params:{
            page: this.pagination.page,
            rows: this.pagination.rowsPerPage,
            sortBy: this.pagination.sortBy,
            desc: this.pagination.descending,
            key: this.key
          }
        }).then(resp => {
          this.brands = resp.data.items;
          this.totalBrands = resp.data.total;
          this.loading = false;
        })
      }
    }
  }
</script>

<!-- scoped:当前样式只作用于当前组件的节点 -->
<style scoped>

</style>
