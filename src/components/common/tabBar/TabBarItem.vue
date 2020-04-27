<template>
  <div class="tab-bar-item" @click="itemClick">
    <div v-if="isActive">
      <slot name="item-icon"></slot>
    </div>
    <div v-else>
      <slot name="item-active-icon"></slot>
    </div>
    <div :style="activeStyle">
      <slot name="item-text"></slot>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'TabBarItem',
    props: {
      path: String,
      activeClass: {
        type: String,
        default: 'deeppink'
      }
    },
    methods: {
      itemClick() {
        console.log(this.path);
        this.$router.replace(this.path)
      }
    },
    computed: {
      isActive() {
        return this.$route.path.indexOf(this.path)
      },
      activeStyle() {
        return !this.isActive ? {color: this.activeClass} : {}
      }
    },
  }
</script>

<style scoped>

  .tab-bar-item {
    font-size: 14px;
    flex: 1;
    text-align: center;
    height: 49px;
  }

  .tab-bar-item img {
    width: 24px;
    height: 22px;
    vertical-align: middle;
    margin-bottom: 3px;
  }

</style>
