<template>
  <transition name="move">
  <div class="food-detail" v-show="showFlag" ref="detailWrapper">
   <div class="food-content">
     <div class="image-header">
       <img :src="food.image">
       <div class="back" @click="hide">
       <i class="icon-arrow_lift"></i>
       </div>
     </div>
     <div class="content">
       <h1 class="title">{{food.name}}</h1>
       <div class="detail">
         <span class="sell-count">月售{{food.sellCount}}</span>
         <span class="rating">好评率{{food.rating}}%</span>
       </div>
       <div class="price">
         <span class="now">¥{{food.price}}</span><span v-show="food.oldPrice" class="old">¥{{food.oldPrice}}</span>
       </div>
       <div class="cartcontrol-wrapper">
         <cartcontrol :food="food"></cartcontrol>
       </div>
       <div @click='addFirst' class="buy" v-show="!food.count||food.count === 0">加入购物车</div>
     </div>
     <split></split>
     <div class="info" v-show="food.info">
       <div class="title">
         <h1 class="title">商品信息</h1>
         <p class="text">{{food.info}}</p>
       </div>
     </div>
     <split v-show="food.info"></split>
     <div class="rating">
       <h1 class="title">商品评价</h1>
       <ratingselect :select-type="selectType" :only-content="onlyContent" :desc="desc" :ratings="food.ratings"></ratingselect>
       <div class="ratings-wrapper">
         <ul v-show="food.ratings && food.ratings.length">
           <li v-for="rating in food.ratings" class="rating-item">

           </li>
         </ul>
         <div class="no-rating" v-show="!food.ratings || !food.rating.length"></div>
       </div>
     </div>
   </div>
  </div>
  </transition>
</template>

<script type="text/ecmascript-6">
  const POSITIVE =0;
  const NEGATIVE = 1;
  const ALL = 2;
  import BScroll from 'better-scroll'
  import cartcontrol from 'components/cartcontrol/cartcontrol'
  import split from 'components/split/split'
  import ratingselect from 'components/ratingselect/ratingselect'
  import Vue from "vue"
  export default{
    props:{
      food:{
        type:Object
      }
    },
    data(){
      return {
        showFlag: false,
        selectType:ALL,
        onlyContent:true,
        desc:{
          all:'全部',
          positive:"推荐",
          negative:'吐槽'
        }
      }
    },
    methods:{
      show(){
        this.selectType=ALL;
        this.onlyContent = true;
        this.showFlag = true;
        this.$nextTick(()=>{
           this._initScroll()
        })
      },
      _initScroll(){
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.detailWrapper,{
            click: true
          });
        } else {
          this.scroll.refresh()
        }
      },
      hide(){
        this.showFlag = false;
      },
      addFirst(event){
         if(!event._constructed){
             return;
         }
         Vue.set(this.food,'count',1)
         this.$root.eventHub.$emit('cart.add', event.target)
      }
    },
    components: {
      cartcontrol,
      split,
      ratingselect
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .food-detail
    position fixed
    left 0
    top 0
    bottom 48px
    z-index 30
    width 100%
    background #fff
    transition all 0.4s ease
    &.move-enter-active,&.move-leave-active{
      transform translate3d(0,0,0)
    }
    &.move-enter,&.move-leave-active{
      transform translate3d(100%,0,0)
    }
    .food-content
      .image-header
        position relative
        width 100%
        height 0
        /*设置为100%值时，是相对于盒子的宽度计算的*/
        padding-top 100%
        img
         position absolute
         top 0
         left 0
         width 100%
         height 100%
        .back
          position absolute
          top 10px
          left 0
          .icon-arrow_lift
            display block
            padding 10px
            font-size 20px
            color #fff
      .content
        padding 18px
        position relative
        .title
          line-height 14px
          margin-bottom 8px
          font-size 14px
          font-weight 700
          color rgb(7,17,27)
        .detail
          margin-bottom 18px
          line-height 10px
          height 10px
          font-size 0
          .sell-count,.rating
            font-size 10px
            color rgb(147,153,159)
          .sell-count
            margin-right 12px
        .price
          font-weight 700
          line-height 24px
          .now
            margin-right 8px
            font-size 14px
            color rgb(240,20,20)
          .old
            text-decoration line-through
            font-size 10px
            color rgb(147,153,159)
      .cartcontrol-wrapper
        position absolute
        right 12px
        bottom 12px
      .buy
        position absolute
        right 18px
        bottom 18px
        z-index 10
        height 24px
        line-height 24px
        padding 0 12px
        box-sizing border-box
        font-size 10px
        border-radius 12px
        background rgb(0,160,220)
        color #fff
      .info
        padding 18px
        .title
          line-height 14px
          margin-bottom 6px
          font-size 14px
          color rgb(7,17,27)
        .text
          line-height 24px
          padding 0 8px
          font-size 12px
          color rgb(77,85,93)
      .rating
        padding-top 18px
        .title
          margin-left 18px
          line-height 14px
          font-size 14px
          color rgb(7,17,27)
</style>
