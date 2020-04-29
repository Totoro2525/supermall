<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
    <Scroll class="content" ref="scroll">
      <HomeSwiper :banners="banners"/>
      <HomeRecommend :recommends="recommends"/>
      <HomeFeature :features="features"/>
      <TabControl class="tab-control" :titles="['流行','新款','精选']"
                  @tabClick="tabClick"/>
      <GoodsList :goods="showGoods"/>
    </Scroll>
    <BackTop @click.native="backClick"/>
  </div>
</template>

<script>
  import HomeSwiper from "./childComps/HomeSwiper";
  import HomeRecommend from "./childComps/HomeRecommend";
  import HomeFeature from "./childComps/HomeFeature";


  import NavBar from "../../components/common/navBar/NavBar";
  import TabControl from "../../components/content/tabControl/TabControl";
  import GoodsList from "../../components/content/goods/GoodsList";
  import Scroll from "../../components/common/scroll/Scroll";
  import BackTop from "../../components/content/backTop/BackTop";


  import {getHomeMultidata, getHomeGoods} from "../../network/home";

  export default {
    name: "Home",
    components: {
      HomeSwiper,
      HomeRecommend,
      HomeFeature,

      NavBar,
      TabControl,
      GoodsList,
      Scroll,
      BackTop

    },
    data() {
      return {
        banners: [],
        recommends: [],
        features: [],
        type: 'pop',
        goods: {
          'pop': {page: 0, list: []},
          'new': {page: 0, list: []},
          'sell': {page: 0, list: []}
        }
      }
    },
    methods: {
      /**
       * 事件监听相关的方法
       **/
      tabClick(index) {
        switch (index) {
          case 0:
            this.type = 'pop';
            break;
          case 1 :
            this.type = 'new';
            break;
          case 2 :
            this.type = 'sell';
            break;
        }
      },
      backClick() {
        console.log('backClick');
        this.$refs.scroll.scrollTo(0,0,1000);
      },
      /**
       * 网络请求相关
       **/
      getHomeMultidata() {
        getHomeMultidata().then(res => {
          this.banners = res.data.banner.list;
          this.recommends = res.data.recommend.list;
        })
      },
      getHomeGoods(type) {
        const page = this.goods[type].page + 1;
        getHomeGoods(type, page).then(res => {
          this.goods[type].list.push(...res.data.list);
          this.goods[type].page += 1;
        })
      }
    },
    computed: {
      showGoods() {
        return this.goods[this.type].list;
      }
    },
    created() {
      //1.请求多个数据
      this.getHomeMultidata();
      //2.请求商品数据
      this.getHomeGoods('pop', 1);
      this.getHomeGoods('new', 1);
      this.getHomeGoods('sell', 1);
    }
  }
</script>

<style scoped>
  #home {
    /*padding-top: 44px;*/
    height: 100vh;
    position: relative;
  }

  .home-nav {
    background-color: var(--color-tint);
    color: #fff;

    position: fixed;
    left: 0;
    right: 0;
    top: 0;
    z-index: 9;
  }

  .tab-control {
    position: sticky;
    top: 44px;
    z-index: 9;
  }

  .content {
    /*height: 300px;*/
    overflow: hidden;
    position: absolute;
    top: 44px;
    bottom: 49px;
  }

  /*.content {*/
  /*  height: calc(100% - 50px);*/
  /*  overflow: hidden;*/
  /*  margin-top: 44px;*/
  /*}*/
</style>
