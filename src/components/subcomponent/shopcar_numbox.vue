<template>
   <div class="mui-numbox" data-numbox-min='1' style="height:30px;">
      <button class="mui-btn mui-btn-numbox-minus" type="button">-</button>
      <input id="test" class="mui-input-numbox" type="number" :value="initcount" @change="countChanged" ref="numbox" readonly />
      <button class="mui-btn mui-btn-numbox-plus" type="button">+</button>
   </div>

</template>

<script>
   import mui from '../../lib/mui/js/mui.min.js'

   export default {
      mounted() {
         //初始化数字选择框组件
         mui('.mui-numbox').numbox()
      },
      methods: {
         countChanged() {
            // 数量改变
            // 每当数量值改变 立即把最新的数据同步到购物车的 store中 覆盖之前的数值
            this.$store.commit("updateGoodsInfo", { id: this.id, count: this.$refs.numbox.value })
         }
      },
      props: ["initcount", "goodsid"]
   }
</script>

<style>
   
</style>