<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script type="text/ecmascript-6">
  import header from 'components/header/header.vue'
  import axios from 'axios'
export default {
  data() {
    return {
      seller: {}
    }
  },
  created() {
    axios.get('../static/date.json').then((res) => {
//  在vue1.x的时候，vue的官方推荐HTTP请求工具是vue-resource，但是在vue2.0的时候将推荐工具改成了axios。
//  使用方式都差不多，但需要注意的是：接口返回的res并不直接是返回的数据，而是经过axios本身处理过的json对象。真正的数据在res.data里：
      this.seller = res.data.seller
    })
  },
  components: {
    'v-header': header
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "common/stylus/mixin.styl"
  @import "common/stylus/index.styl"
  .tab
    display: flex
    width: 100%
    height: 40px
    line-height: 40px
    border-1px(rgba(7,17,27,0.1))
    .tab-item
      flex: 1
      text-align: center
      & > a
        display: block
        font-size: 14px
        color: rgb(77, 85, 93)
        &.active
           color red
</style>
<!--#app .tab{-->
<!--display: flex;-->
<!--width: 100%;-->
<!--height: 40px;-->
<!--line-height: 40px;-->
<!--/*设置1px的时候要注意设备的dpi，物理像素是设备像素的两倍,解决方法如下*/-->
<!--/*border-bottom: 1px solid rgba(7,17,27,0.1);*/-->
<!--border-1px(rgba(7,17,27,0.1));-->
<!--}-->
<!--.tab-item{-->
<!--flex: 1;-->
<!--text-align: center;-->
<!--}-->
