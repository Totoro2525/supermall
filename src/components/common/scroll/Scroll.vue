<template>
  <div class="wrapper" ref="wrapper">
    <div class="content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
  import BScroll from 'better-scroll';

  export default {
    name: "Scroll",
    props: {
      probeType: {
        type: Number,
        default: 0
      },
      pullUpLoad: {
        type: Boolean,
        default: false
      },
    },
    data() {
      return {
        scroll: null
      }
    },
    mounted() {
      //1.创建BScroll对象
      this.scroll = new BScroll(this.$refs.wrapper, {
        click: true,
        probeType: this.probeType,
        //上拉加载
        pullUpLoad: this.pullUpLoad,
        // pullUpLoad: {
        //   // 当上拉距离超过30px时触发 pullingUp 事件
        //   threshold: -30,
        //   //回弹停留在距离底部20px的位置
        //   // stop: 10
        // },
        // //下拉刷新
        // pullDownRefresh: {
        //   //下拉距离超过30px触发pullingDown事件
        //   threshold: 30,
        //   //回弹停留在距离顶部20px的位置
        //   stop: 5
        // }
      })
      //2.监听滚动的位置
      if (this.probeType === 2 || this.probeType === 3) {
        this.scroll.on('scroll', (position) => {
          this.$emit('scroll', position);
        })
      }
      //3.监听scroll滚动到底部
      if (this.pullUpLoad) {
        this.scroll.on('pullingUp', () => {
          this.$emit('pullingUp')
        })
        // this.scroll.on('pullingDown', () => {
        //   console.log('处理下拉刷新操作')
        //   setTimeout(() => {
        //     // 事情做完，需要调用此方法告诉 better-scroll 数据已加载，否则下拉事件只会执行一次
        //     this.scroll.finishPullDown()
        //   }, 2000)
        // })
      }
    },
    methods: {
      scrollTo(x, y, time = 500) {
        this.scroll && this.scroll.scrollTo(x, y, time);
      },
      finishPullUp() {
        this.scroll && this.scroll.finishPullUp()
      },
      refresh() {
        this.scroll && this.scroll.refresh()
      }
    }
  }
</script>

<style scoped>

</style>
