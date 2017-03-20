<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="(item,index) in goods" class="menu-item" :class="{'current':menuCurrentIndex===index}">
          <span class="text border-1px" @click="selectMenu(index,$event)">
          <iconMap v-show="item.type>0" :iconType="item.type"></iconMap>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <li v-for="item in goods" class="food-list food-list-hook">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li @click="foodList(food,$event)" v-for="food in item.foods" class="food-item border-1px">
              <div class="icon">
                <img :src="food.icon" width="57" height="57">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">¥{{food.price}}</span><span v-show="food.oldPrice" class="old">¥{{food.oldPrice}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cartcontrol :food="food"></cartcontrol>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart :deliveryPrice="seller.deliveryPrice" :minPrice = "seller.minPrice" :selectFoods="selectFoods"></shopcart>
    <food :food="selectedFood" v-if="selectedFood" ref="food"></food>
  </div>

</template>

<script type="text/ecmascript-6">
  import iconMap from 'components/iconMap/iconMap';
  import axios from 'axios';
  import Bscroll from  'better-scroll';
  import shopcart from 'components/shopcart/shopcart'
  import cartcontrol from 'components/cartcontrol/cartcontrol'
  import food from 'components/food/food'


  export default{
    props:{
      seller:{
        type:Object
      }
      },
    created(){
      axios.get('/static/date.json').then((res) => {
        this.goods = res.data.goods;
//        dom发生变化实在nexttick后
        this.$nextTick(() => {
          this._initScroll(); // 初始化scroll
          this._calculateHeight(); // 初始化列表高度列表
        })
      });
    },
    data(){
      return {
        goods:[],
        listHeight:[],
        scrollY:0,
        selectedFood: ''
      }
    },
    methods:{
      selectMenu(index,event){
//          VUE自带的事件中用constructed属性而浏览器原生事件中不带，所以解决了在pc端多次派发时间的bug
        if(!event._constructed){
            return
        }
        this.foodsScroll.scrollTo(0, -this.listHeight[index], 300)
      },
      _initScroll(){
        this.menuScroll = new Bscroll(this.$refs.menuWrapper,{
          click:true
        });
        this.foodsScroll = new Bscroll(this.$refs.foodsWrapper, {
          click:true,
          probeType: 3
        });
//        监听滚动属性
        this.foodsScroll.on('scroll',(pos)=>{
            this.scrollY = Math.abs(Math.round(pos.y));
        })
      },
      _calculateHeight() {
        let foodList = this.$refs.foodsWrapper.querySelectorAll('.food-list-hook');
        let height = 0;
        this.listHeight.push(height);
        for (let i = 0, l = foodList.length; i < l; i++) {
          let item = foodList[i];
          height += item.clientHeight;
          this.listHeight.push(height);
        }
      },
      foodList(food,event){
        if(!event._constructed){
          return
        }
        this.selectedFood = food;
        this.$nextTick(() => {
          this.$refs.food.show();
        })
      }
    },
    computed:{
      menuCurrentIndex(){
        for (let i = 0, l = this.listHeight.length; i < l; i++) {
          let topHeight = this.listHeight[i];
          let bottomHeight = this.listHeight[i + 1];
          if (!bottomHeight || (this.scrollY >= topHeight && this.scrollY < bottomHeight)) {
            return i
          }
        }
        return 0
      },
      selectFoods(){
        let foods = [];
        this.goods.forEach((good)=>{
            good.foods.forEach((food)=>{
                if(food.count){
                    foods.push(food)
                }
            });
        });
        return foods;
      }
    },
    components:{
      iconMap,
      shopcart,
      cartcontrol,
      food
    }
  };

</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import '../../common/stylus/mixin.styl'
  .goods
    display flex
    position absolute
    width 100%
    top 174px
    bottom 46px
    overflow hidden
    .menu-wrapper
      flex 0 0 80px
      /*不写80的话安卓手机会出现问题*/
      width 80px
      background #f3f5f7
      .menu-item
        /*多行文字垂直居中*/
        display table
        height 54px
        width 56px
        line-height 14px
        padding 0 12px
        &.current
          position relative
          z-index 10
          margin-top -1px
          background #fff
          font-weight 700
          .text
            border-none()
        .text
          display table-cell
          width 56px
          vertical-align middle
          font-size 12px
          border-1px(rgba(7,17,27,0.2))
    .foods-wrapper
      flex 1
      .title
        padding-left 14px
        height 26px
        line-height 26px
        border-left 1px solid #d9dde1
        font-size 12px
        color rgb(147,153,159)
        background #f3f5f7
      .food-item
        display flex
        margin 18px
        padding-bottom 18px
        border-1px(rgba(7,17,27,0.2))
        &:last-child
          border-none()
          margin-bottom 0
        .icon
          flex 0 0 57px
          margin-right 10px
        .content
          flex 1
          .name
            margin 2px 0 8px 0
            height 14px
            line-height 14px
            font-size 14px
            color rgb(7,17,27)
          .desc,.extra
             line-height 10px
             font-size 10px
             color rgb(147,153,159)
          .desc
            margin-bottom 8px
            line-height 12px
          .extra
            .count
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
            right 0
            bottom 12px
</style>
