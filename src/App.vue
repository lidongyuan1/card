<template>
  <div class="app-container">
    <Header title="购物车案例"></Header>
    <h1>{{amt}}</h1>
    <Goods v-for="item in list" 
        @state-change="getNewState"
        :key="item.id"
        :id="item.id" 
        :pic="item.goods_img" 
        :title="item.goods_name" 
        :state="item.goods_state" 
        :price="item.goods_price"
        :count="item.goods_count">
    </Goods>
    <Footer :isFull="fullState" @fullChange="getFullState" 
      :amout="amt" :all="total" 
    ></Footer>
  </div>
</template>
<!-- 一.基于axios请求列表数据 -->
<!-- 1.导入axios -->
<!-- 2.在methods方法中，定义initCartList函数请求列表数据 -->
<!-- 3.在creat生命函数中，调用步骤二中的方法 -->
<script>
// 导入axios请求库
  import axios from 'axios'
// 导入需要的组件
  import Header from '@/components/Header/Header.vue'
  import Goods from "@/components/Goods/Goods.vue";
  import Footer from "@/components/Footer/Footer.vue";
  import bus from "@/components/eventBus"
export default {
  created(){
    // 调用请求数据的方法
    this.initCartList(),
    bus.$on('share',val=>{
      this.list.some(item =>{
        if(item.id ===val.id){
          item.goods_count =val.value
          return true
        }
      })
        
    })
  },
  data() {
    return {
      //用于储存购物车的列表数据，默认为空数组
      list:[]
    }
  },
  computed: {
    fullState(){
      return this.list.every(item=>item.goods_state===true)
    },
    //计算勾选商品的总价格
    amt(){
   return  this.list.filter(item =>item.goods_state)
      .reduce((total,item)=>{
        return  total+=item.goods_price*item.goods_count
      },0)

    },
    //计算已勾选商品的总数量
    total(){
      return this.list.filter(item=>item.goods_state)
      .reduce((t,item)=>{
       return t += item.goods_count
      },0)
    }
  },
  methods:{
  // 封存请求列表数据的方法
    async initCartList(){
      const {data:res} = await axios.get('https://www.escook.cn/api/cart')
      console.log(res);
      //只要请求回来的数据，在页面渲染期间要用到，则必须转存到data中
      if(res.status === 200){
        this.list = res.list
      }
    },
    //获取从子组件中传来的数据
    getNewState(e){
        this.list.some(item=>{
          if(item.id ===e.id){
            item.goods_state = e.val
            //终止循环
            return true 
          }
        })
    },
    getFullState(val){
      this.list.forEach(item => {
        item.goods_state = val
      });
    }
  },
  components:{
     Header,
     Goods,
     Footer
  }
  
}
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
