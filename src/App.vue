<template>
  <div class="app-container">
    <!-- Header 头部区域-->
    <Header></Header>


    <!--循环渲染每个Goods-->
    <Goods v-for="item in cartList" :key="item.id" 
    :title="item.goods_name" 
    :pic="item.goods_img" 
    :price="item.goods_price" 
    :state="item.goods_state"
    :id="item.id"
    :count="item.goods_count"
    @getNewState="changeState"
    ></Goods>

    <!--Footer组件-->
    <Footer 
    :fullState="chooseAll"
    :amount="totalPrice"
    :totalCount="totalCount"
    @fullChange="getNewFullChange"

    ></Footer>
  </div>
</template>

<script>
import Header from '@/components/Header/Header.vue'
import Goods from '@/components/Goods/Goods.vue'
import Footer from '@/components/Footer/Footer.vue'
import bus from '@/components/EventBus.js'
import axios from 'axios'
  export default {
    data(){
      return{
        cartList: []
      }
    },
    methods:{
      async initCartList(){
        const {data:res} = await axios.get('https://www.escook.cn/api/cart')
        // console.log(res)
        if(res && res.status === 200){
          this.cartList = res.list
        }  
      },
      // 接收来自Goods组件单个勾选状态的变化
      changeState(obj){
        // console.log(obj)
        // {id:1, vaule: true}
        this.cartList.some(item => {
          if(item.id == obj.id){
            item.goods_state = obj.value
            return true
          }
        })
      },
      // 接收来自Footer组件的全选状态的变化
      getNewFullChange(val){
        this.cartList.forEach(item => item.goods_state = val)
      }
    },
    components:{
      Header,
      Goods,
      Footer
    },
    created(){
      this.initCartList(),
      bus.$on('shareAdd', val =>{
        this.cartList.some(item => {
          if(item.id == val.id){
            item.goods_count = val.goods_count
            return true
          }
        })
      }),
      bus.$on('shareSub', val =>{
        this.cartList.some(item => {
          if(item.id == val.id){
            item.goods_count = val.goods_count
            return true
          }
        })
      })
    },
    computed:{
      // 选中的商品总价格
      totalPrice(){
        return this.cartList.filter(item => item.goods_state).reduce((amt, item) =>{
          return amt += item.goods_price * item.goods_count
        }, 0)
      },
      // 判断全选状态
      chooseAll(){
        return this.cartList.every(item => item.goods_state)
      },
      // 勾选商品的总数量
      totalCount(){
        return this.cartList.filter(item => item.goods_state).reduce((count, item) => {
          return count += item.goods_count
        }, 0)
      }
    }
  }
</script>


<style lang="less">
  .app-container{
    padding-top: 45px;
    padding-bottom: 50px;
  }
  
</style>