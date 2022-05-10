<template>
  <div class="app-container">
  <!-- header模块-->
    <Header></Header>
    <p>{{amt}}</p>
    <Goods
        v-for="item in list"
        :key="item.id"
        :title="item.goods_name"
        :pic="item.goods_img"
        :price="item.goods_price"
        :state="item.goods_state"
        :id="item.id"
        :count="item.goods_count"
        @state-change="getNewState"
    ></Goods>
    <Footer :isFull="fullState" :Sum="amt" :all="total" @full-change="getFullState"></Footer>
  </div>
</template>

<script>
import bus from "@/components/eventBus.js"
//导入axios请求库
import axios from 'axios'
//导入需要的组件
import Header from "@/components/Header/Header.vue";
import Goods from "@/components/Goods/Goods.vue";
import Footer from "@/components/Footer/Footer.vue";
export default {
  data(){
    return {
      //创建的空数组用来存放请求过来的数据
      list:[]
    }
  },
  components :{
    Header,
    Goods,
    Footer
  },
  methods :{
    async initCatList (){
      //调用axios的get方法请求数据
      const {data:res}=await axios.get('https://www.escook.cn/api/cart')
      console.log(res)
      this.list=res.list

    },
    getNewState (val){
      console.log(val)
      this.list.some(item=>{
        if (item.id===val.id){
          item.goods_state=val.val
        }
      })
    },
    getFullState (e){
      this.list.forEach(item=>item.goods_state=e)
    }
  },

  computed:{
    //计算出全选的状态是true还是false
    fullState (){
      return this.list.every(item=>item.goods_state===true)
    },
    amt (){
      return this.list.filter((item)=>item.goods_state===true)
      .reduce((sum,item)=>(sum=sum+item.goods_price*item.goods_count),0)
    },
    total (){
      return this.list.filter((item)=>item.goods_state===true)
      .reduce((t,item)=>{
        return t+=item.goods_count
      },0)

    }
  },


  created() {
    //调用请求数据的方法
    this.initCatList()

    //接收Count组件传过来的值并修改列表数据
    bus.$on('share',(val)=>{
      // console.log(val)
      this.list.some((item)=>{
        if (item.id===val.id){
          item.goods_count=val.value
          return
        }
      })
    })
  }
}
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
