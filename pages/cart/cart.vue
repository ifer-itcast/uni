<template>
  <view>
    <!-- 购物车商品列表的标题区域 -->
    <view class="cart-title">
      <!-- 左侧的图标 -->
      <uni-icons type="shop" size="18"></uni-icons>
      <!-- 描述文本 -->
      <text class="cart-title-text">购物车</text>
    </view>
    <!-- 商品列表区域 -->
    <my-swipe-action>
      <block v-for="(goods, i) in cart" :key="i">
        <!-- my-swipe-action-item 可以为其子节点提供滑动操作的效果。需要通过 options 属性来指定操作按钮的配置信息 -->
        <my-swipe-action-item :options="options" @click="swipeActionClickHandler(goods)">
          <my-goods :goods="goods" :show-radio="true" :show-num="true" @radio-change="radioChangeHandler"
            @num-change="numberChangeHandler"></my-goods>
        </my-swipe-action-item>
      </block>
    </my-swipe-action>
  </view>
</template>

<script>
  // 按需导入 mapState 这个辅助函数
  import {
    mapGetters,
    mapState,
    mapMutations
  } from 'vuex'
  export default {
    data() {
      return {
        options: [{
          text: '删除', // 显示的文本内容
          style: {
            backgroundColor: '#C00000' // 按钮的背景颜色
          }
        }]
      }
    },
    computed: {
      // 将 m_cart 模块中的 total 映射为当前页面的计算属性
      ...mapGetters('m_cart', ['total']),
      ...mapState('m_cart', ['cart']),
    },
    onShow() {
      // 在页面刚展示的时候，设置数字徽标
      this.setBadge()
    },
    methods: {
      ...mapMutations('m_cart', ['updateGoodsState', 'updateGoodsCount', 'removeGoodsById']),
      setBadge() {
        // 调用 uni.setTabBarBadge() 方法，为购物车设置右上角的徽标
        uni.setTabBarBadge({
          index: 2, // 索引
          text: this.total + '' // 注意：text 的值必须是字符串，不能是数字
        })
      },
      ...mapMutations('m_cart', ['updateGoodsState']),
      radioChangeHandler(e) {
        this.updateGoodsState(e) // 输出得到的数据 -> {goods_id: 395, goods_state: false}
      },
      numberChangeHandler(e) {
        this.updateGoodsCount(e)
      },
      swipeActionClickHandler(goods) {
          this.removeGoodsById(goods.goods_id)
        }
    }
  }
</script>

<style lang="scss">
  .cart-title {
    height: 40px;
    display: flex;
    align-items: center;
    font-size: 14px;
    padding-left: 5px;
    border-bottom: 1px solid #efefef;

    .cart-title-text {
      margin-left: 10px;
    }
  }
</style>
