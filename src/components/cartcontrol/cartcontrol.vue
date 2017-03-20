<template>
  <div class="cartcontrol">
    <transition name="fadeRotate">
    <div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="decreaseCart">
      <span class="inner icon-remove_circle_outline"></span>
    </div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click.stop.prevent="addCart"></div>
  </div>

</template>

<script type="text/ecmascript-6">
// 当给观测对象添加一个不存在的字段的时候，不可以直接赋值的，因为检测不到新增的数据变化
// 如果需要增加字段的时候需要用到vue.set的方法
  import vue from 'vue';
  export default{
    props:{
      food:{
        type:Object
      }
    },
    methods:{
      addCart (event){
        if(!event._constructed){
          return
        }
        if(!this.food.count){
          vue.set(this.food,'count',1)
        }else{
            this.food.count++;
        }
//        派发事件 vue1中的$dispatch 和 $broadcast被废弃
        this.$root.eventHub.$emit('cart.add', event.target)
      },
      decreaseCart(event){
        if(!event._constructed){
          return
        }
        if(this.food.count) {
          this.food.count--;
        }
      }
    }
  }

</script>

<style lang="stylus" rel="stylesheet/stylus">
  .cartcontrol
    font-size 0
    .cart-decrease, .cart-add
      display inline-block
      padding 6px
      transition all 0.4s linear
      .inner
        font-size 24px
        line-height 24px
        color rgb(0,160,220)
        transition all 0.4s linear
      &.fadeRotate-enter-active, &.fadeRotate-leave-active
        transform translate3d(0,0,0)
        .inner
          display inline-block
          transform rotate(0)
      &.fadeRotate-enter, &.fadeRotate-leave-active
        opacity: 0
        transform translate3d(24px,0,0)
        .inner
          transform rotate(180deg)
    .cart-count
      display inline-block
      vertical-align top
      width 12px
      padding-top 6px
      font-size 10px
      color rgb(147,153,159)
      line-height 24px
      text-align center
    .cart-add
      display inline-block
      font-size 24px
      line-height 24px
      padding 6px
      color rgb(0,160,220)

</style>
