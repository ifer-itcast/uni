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
        kw: ''
			};
		},
    methods: {
      input(e) {
        clearTimeout(this.timer)
          // 重新启动一个延时器，并把 timerId 赋值给 this.timer
          this.timer = setTimeout(() => {
            // 如果 500 毫秒内，没有触发新的输入事件，则为搜索关键词赋值
            this.kw = e
            console.log(this.kw)
          }, 500)
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
