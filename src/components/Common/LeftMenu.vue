<template>
<div class="left-nav">
  <ul>
    <li>
      <i class="icon iconfont icon-pay"></i>
      <div>
        <router-link to="/">点餐</router-link>
      </div>
    </li>
    <li>
      <i class="icon iconfont icon-index"></i>
      <div><router-link to="/test">店铺</router-link></div>
    </li>
    <li>
      <i class="icon iconfont icon-icon1"></i>
      <div>商品</div>
    </li>
    <li>
      <i class="icon iconfont icon-myself"></i>
      <div>会员</div>
    </li>
    <li>
      <i class="icon iconfont icon-list"></i>
      <div>统计</div>
    </li>
  </ul>
</div>
</template>
<script>
  import router from '../../router'
  export default {
    router,
    name:'leftMenu',
    data() {
      return {

      }
    }
}
</script>
<style scoped>
  .left-nav{
    color:#fff;
    font-size:10px;
    height:100%;
    background-color:#1d8ce0;
    float:left;
    width:5%;
  }
  .iconfont{
    font-size:24px;
  }
  .left-nav li{
    text-align:center;
    border-bottom:1px solid #20a0ff;
    padding:10px;
  }
</style>
