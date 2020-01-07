<template>
    <div>
      this is form
      <v-form v-model="valid">
        <v-text-field label="请输入品牌名称" required=""/>
        <v-text-field label="请输入首字母" required=""/>
        <v-cascader
          url="/item/category/list"
          multiple
          required
          v-model="brand.categories"
          label="请选择商品分类"/>
        <v-layout row>
          <v-flex xs3>
            <span style="font-size: 16px; color: #444">品牌LOGO：</span>
          </v-flex>
          <v-flex>
            <v-upload
              v-model="brand.image"
              url="/upload"
              :multiple="false"
              :pic-width="250"
              :pic-height="90"
            />
          </v-flex>
        </v-layout>
        <v-layout class="my-4" row>
          <v-btn @click="submit"color="primary">提交</v-btn>
          <v-spacer/>
          <v-btn @click="clear()" color="warning">重置</v-btn>
        </v-layout>
      </v-form>
    </div>
</template>

<script>
    export default {
      name: "MyBrandForm",
      data(){
          return{
            valid: false,
            brand:{
              name:'',
              letter:'',
              image:'',
              categories:[]
            }

          }
      }
    }
</script>

<style scoped>

</style>
