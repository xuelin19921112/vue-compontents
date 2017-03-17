<!-- 配件详情组件 -->
<template>
	<div class="app">
		<div v-show="!showinquery">
			<!-- 页面主体 -->
			<goods-info-main :prodInfo="prodInfo"></goods-info-main>
		</div>
		<div v-show="showinquery">
           
		</div>
	</div>
</template>

<script>
	import Vue from 'vue'
	import goodsInfoMain from '../goodsInfo/goodsInfo_main.vue'

	export default {
		name: 'goodsInfo',
		components: {
			goodsInfoMain
		},
		data() {
			return {
               showinquery:false,
			   prodInfo:{}
			}
		},
		beforeRouteEnter(to, from, next) {
			next(vm => {
				vm.prodInfo=vm.$route.params.prodInfo;
				console.log(1);
			})
		},
		methods: {

		}
	}

</script>

<style>

</style>