<template>
  <tab-bar>
    <tab-bar-item path="/home" activeClass="deeppink">
      <img slot="item-icon" src="../../../assets/img/tabbar/home.svg" alt="">
      <img slot="item-active-icon" src="../../../assets/img/tabbar/home_active.svg" alt="">
      <div slot="item-text">首页</div>
    </tab-bar-item>
    <tab-bar-item path="/category" activeClass="deeppink">
      <img slot="item-icon" src="../../../assets/img/tabbar/category.svg" alt="">
      <img slot="item-active-icon" src="../../../assets/img/tabbar/category_active.svg" alt="">
      <div slot="item-text">分类</div>
    </tab-bar-item>
    <tab-bar-item path="/cart" activeClass="deeppink">
      <img slot="item-icon" src="../../../assets/img/tabbar/shopcart.svg" alt="">
      <img slot="item-active-icon" src="../../../assets/img/tabbar/shopcart_active.svg" alt="">
      <div slot="item-text">购物车</div>
    </tab-bar-item>
    <tab-bar-item path="/profile" activeClass="deeppink">
      <img slot="item-icon" src="../../../assets/img/tabbar/profile.svg" alt="">
      <img slot="item-active-icon" src="../../../assets/img/tabbar/profile_active.svg" alt="">
      <div slot="item-text">我的</div>
    </tab-bar-item>
  </tab-bar>
</template>

<script>
  import TabBar from "../../common/tabBar/TabBar";
  import TabBarItem from "../../common/tabBar/TabBarItem";

  export default {
    name: "MainTabBar",
    components: {
      TabBar,
      TabBarItem
    }
  }
</script>

<style scoped>

</style>
