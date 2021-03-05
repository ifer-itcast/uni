<template>
	<view class="search-box">
	  <!-- 使用 uni-ui 提供的搜索组件 -->
	  <uni-search-bar @input="input" :radius="100" cancelButton="none"></uni-search-bar>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 延时器的 timerId
        timer: null,
        // 搜索关键词
        kw: '',
        searchResults: []
			};
		},
    methods: {
      input(e) {
        clearTimeout(this.timer)
          // 重新启动一个延时器，并把 timerId 赋值给 this.timer
          this.timer = setTimeout(() => {
            // 如果 500 毫秒内，没有触发新的输入事件，则为搜索关键词赋值
            this.kw = e
            this.getSearchList()
          }, 500)
      },
      async getSearchList() {
        // 判断关键词是否为空
        if (this.kw === '') {
          this.searchResults = []
          return
        }
        // 发起请求，获取搜索建议列表
        const { data: res } = await uni.$http.get('/api/public/v1/goods/qsearch', { query: this.kw })
        if (res.meta.status !== 200) return uni.$showMsg()
        this.searchResults = res.message
      }
    }
	}
</script>

<style lang="scss">
.search-box {
  position: sticky;
  top: 0;
  z-index: 999;
}
</style>
