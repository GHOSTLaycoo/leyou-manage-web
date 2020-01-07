<template>
  <v-card>
      <v-flex xs12 sm10>
        <v-tree url="/item/category/list"
                :isEdit="isEdit"
                @handleAdd="handleAdd"
                @handleEdit="handleEdit"
                @handleDelete="handleDelete"
                @handleClick="handleClick"
        />
      </v-flex>
  </v-card>
</template>

<script>
  export default {
    name: "category",
    data() {
      return {
        isEdit:true
      }
    },
    methods: {
      handleAdd(node) {
        console.log("add .... ");
        console.log(node);
      },
      handleEdit(id, name) {
        this.$http.put("/item/category",{
          params: {
            id: id, // 分类ID
            name: name // 分类名称
          }
        }).then(function(resp) { // 这里使用箭头函数
            this.name = resp.data.name;
          }
          );
        console.log("edit... id: " + id + ", name: " + name)
      },
      handleDelete(id) {
        console.log("delete ... " + id)
      },
      handleClick(node) {
        // this.$http.put("/item/category",{
        //   params: {
        //     id: node.id, // 分类ID
        //     name: node.name // 分类名称
        //   }
        // }).then(function(resp) { // 这里使用箭头函数
        //     this.name = resp.data.name;
        //   }
        // );
        //console.log(node)
      }
    }
  };
</script>

<style scoped>

</style>
