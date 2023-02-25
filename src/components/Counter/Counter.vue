<template>
  <div class="number-container d-flex justify-content-center align-items-center">
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="sub">-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{ goods_number }}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="add">+</button>
  </div>
</template>

<script>
import bus from '../EventBus.js'
export default {
  props:{
    // 商品数量
    goods_number:{
      type: Number,
      default: 1
    },
    // 商品id，向App传递数据时使用
    id: {
      type: Number,
      required: true
    }
  },
  methods:{
    // 商品数量+1
    add(){
      bus.$emit('shareAdd', {id: this.id, goods_count: this.goods_number + 1})
    },
    // 商品数量-1
    sub(){
      if(this.goods_number - 1 < 0) return
      bus.$emit('shareSub', {id: this.id, goods_count: this.goods_number - 1})
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
