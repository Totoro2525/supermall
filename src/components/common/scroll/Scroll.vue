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
    data() {
      return {
        scroll: null
      }
    },
    mounted() {
      this.scroll = new BScroll(this.$refs.wrapper, {
        prototype: 3,
        //上拉加载
        pullUpLoad: {
          // 当上拉距离超过30px时触发 pullingUp 事件
          threshold: -30,
          //回弹停留在距离底部20px的位置
          // stop: 10
        },
        //下拉刷新
        pullDownRefresh: {
          //下拉距离超过30px触发pullingDown事件
          threshold: 30,
          //回弹停留在距离顶部20px的位置
           stop: 5
        }
      })
      this.scroll.on('scroll', (position) => {
        console.log(position);
      })
      this.scroll.on('pullingUp', () => {
        console.log('处理上拉加载动作');
        setTimeout(() => {
          console.log('数据已经加载完毕');
          //事情做完，需要调用此方法告诉 better-scroll 数据已加载，否则上拉事件只会执行一次
          this.scroll.finishPullUp()
        }, 2000)
      })
      this.scroll.on('pullingDown', () => {
        console.log('处理下拉刷新操作')
        setTimeout(() => {
          // 事情做完，需要调用此方法告诉 better-scroll 数据已加载，否则下拉事件只会执行一次
          this.scroll.finishPullDown()
        }, 2000)
      })
      this.scroll.scrollTo(0,0,500)
    },
    methods: {
      scrollTo(x,y,time=500){
        this.scroll.scrollTo(x,y,time);
      }
    }
  }
</script>

<style scoped>

</style>
