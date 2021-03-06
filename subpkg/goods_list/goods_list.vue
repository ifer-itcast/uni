<template>
	<view>
		<view class="goods-list">
		  <block v-for="(item, i) in goodsList" :key="i">
		    <!-- 为 my-goods 组件动态绑定 goods 属性的值 -->
		    <my-goods :goods="item"></my-goods>
		  </block>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				queryObj: {
          // 查询关键词
          query: '',
          // 商品分类Id
          cid: '',
          // 页码值
          pagenum: 1,
          // 每页显示多少条数据
          pagesize: 10
        },
        // 商品列表的数据
        goodsList: [],
        // 总数量，用来实现分页
        total: 0,
        isloading: false
			};
		},
    onLoad(options) {
      // 将页面参数转存到 this.queryObj 对象中
      this.queryObj.query = options.query || ''
      this.queryObj.cid = options.cid || ''
      this.getGoodsList()
    },
    methods: {
      async getGoodsList() {
        this.isloading = true
        // 发起请求
        const { data: res } = await uni.$http.get('/api/public/v1/goods/search', this.queryObj)
        this.isloading = false
        if (res.meta.status !== 200) return uni.$showMsg()
        // 为数据赋值
        this.goodsList = [...this.goodsList, ...res.message.goods]
        this.total = res.message.total
      }
    },
    onReachBottom() {
       // 判断是否正在请求其它数据，如果是，则不发起额外的请求
        if (this.isloading) return

      // 让页码值自增 +1
      this.queryObj.pagenum += 1
      // 重新获取列表数据
      this.getGoodsList()
    }
	}
</script>
