<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
    <Scroll class="content" ref="scroll" :probe-type="3" @scroll="contentScroll" :pull-up-load="true"
            @pullingUp="loadMore">
      <HomeSwiper :banners="banners" @swiperImageLoad="swiperImageLoad"/>
      <HomeRecommend :recommends="recommends"/>
      <HomeFeature/>
      <TabControl :titles="['流行','新款','精选']"
                  @tabClick="tabClick" ref="tabControl" :class="{fixed:isTabFixed}"/>
      <GoodsList :goods="showGoods"/>
    </Scroll>
    <BackTop @click.native="backClick" v-show="isShowBackTop"/>
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

  import {debounce} from "../../common/utils";

  import {getHomeMultiData, getHomeGoods} from "../../network/home";

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
        type: 'pop',
        goods: {
          'pop': {page: 0, list: []},
          'new': {page: 0, list: []},
          'sell': {page: 0, list: []}
        },
        isShowBackTop: false,
        tabOffsetTop: 0,
        isTabFixed: false
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
      }
      ,
      backClick() {
        console.log('backClick');
        this.$refs.scroll.scrollTo(0, 0);
      }
      ,
      contentScroll(position) {
        // 1.判断BackTop是否显示
        this.isShowBackTop = position.y < -1000;

        // 2.决定tabControl是否吸顶(position: fixed)
        this.isTabFixed = (-position.y) > this.tabOffsetTop
      }
      ,
      loadMore() {
        console.log('加载下一页');
        this.getHomeGoods(this.type)
      },
      swiperImageLoad() {
        //2.获取tabControl的offsetTop
        console.log(this.$refs.tabControl.$el.offsetTop);
        this.tabOffsetTop = this.$refs.tabControl.$el.offsetTop
      },
      /**
       * 网络请求相关
       **/
      getHomeMultiData() {
        getHomeMultiData().then(res => {
          this.banners = res.data.banner.list;
          this.recommends = res.data.recommend.list;
        })
      },
      getHomeGoods(type) {
        const page = this.goods[type].page + 1
        getHomeGoods(type, page).then(res => {
          this.goods[type].list.push(...res.data.list)
          this.goods[type].page += 1
          console.log('页数' + this.goods[type].page);
          // 完成上拉加载更多
          this.$refs.scroll.finishPullUp()
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
      this.getHomeMultiData();
      //2.请求商品数据
      this.getHomeGoods('pop', 1);
      this.getHomeGoods('new', 1);
      this.getHomeGoods('sell', 1);

    },
    mounted() {
      //1.图片加载完的事件监听
      const refresh = debounce(this.$refs.scroll.refresh, 50);
      this.$bus.$on('itemImageLoad', () => {
        console.log('-----');
        refresh()
      })
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

  /*.tab-control {*/
  /*  position: sticky;*/
  /*  top: 44px;*/
  /*  z-index: 9;*/
  /*}*/

  .content {
    /*height: 300px;*/
    overflow: hidden;
    position: absolute;
    top: 44px;
    bottom: 49px;
  }
  .fixed{
    position: fixed;
    left: 0;
    right: 0;
    top:44px;
    z-index: 10
  }
  /*.content {*/
  /*  height: calc(100% - 50px);*/
  /*  overflow: hidden;*/
  /*  margin-top: 44px;*/
  /*}*/
</style>
