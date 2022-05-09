<template>
  <div class="app-container">
  <!-- header模块-->
    <Header></Header>
    <Goods v-for="item in list" :key="item.id"></Goods>
    <div></div>
  </div>
</template>

<script>
//导入axios请求库
import axios from 'axios'
//导入需要的组件
import Header from "@/components/Header/Header.vue";
import Goods from "@/components/Goods/Goods.vue";
export default {
  data(){
    return {
      //创建的空数组用来存放请求过来的数据
      list:[]
    }
  },
  components :{
    Header,
    Goods
  },
  methods :{
    async initCatList (){
      //调用axios的get方法请求数据
      const {data:res}=await axios.get('https://www.escook.cn/api/cart')
      console.log(res)
      this.list=res.list

    }
  },

  created() {
    //调用请求数据的方法
    this.initCatList()
  }
}
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
