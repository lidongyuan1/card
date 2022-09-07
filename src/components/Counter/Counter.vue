<template>
  <div class="number-container d-flex justify-content-center align-items-center">
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="sub">-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{num}}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="add">+</button>
  </div>
</template>

<script>
import bus from '../eventBus'
export default {
  props:{
    //接受商品的数量
    num:{
      default:1,
      tpye:Number
    },
    //接受商品的id
    id:{
      default:1,
      required:true
    }
  },
  methods:{
    add(){
      //要发送个app的数据为{id,value}
      //其中id使商品的id，value使商品的最新数量
      //要做的事：通过eventbus把obj对象发送给app
      const obj = {id:this.id,value:this.num+1}
      bus.$emit('share',obj)
    },
    sub(){
      if(this.num ===1) return
      const obj = {id:this.id,value:this.num-1}
      bus.$emit('share',obj)
    }
  }
}
</script>

<style lang="less" scoped>
.number-box {
  min-width: 30px;
  text-align: center;
  margin: 0 5px;
  font-size: 12px;
}

.btn-sm {
  width: 30px;
}
</style>
