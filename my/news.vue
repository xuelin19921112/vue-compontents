<!-- 消息组件 -->
<template>
	<div id="news" class="">
		 <hearder :title="title"></hearder>
		<div class="content">
			<ul>
				<li>
					<div class="mui-card-header xj_header">
						<label>订单</label>
						<span class="font-color-grey">2017-01-07<em>18:21:41</em></span>
					</div>
					<div class="mui-input-row mui-checkbox mui-left">
						<p>尊敬的用户，您于2016.1.21&nbsp15:00:00提交的订单，经销商修改了订单金额，请及时前往查看</p>
					</div>
				</li>
				<li>
					<div class="mui-card-header xj_header">
						<label class="font-color-red">活动消息</label>
						<span class="font-color-grey">2017-01-07<em>18:21:41</em></span>
					</div>
					<div class="mui-input-row mui-checkbox mui-left">
						<p>尊敬的用户，双十一活动正式开启，购物有优惠，千万不要错过哦。请及时前往查看</p>
					</div>
				</li>
			</ul>
		</div>

	</div>
</template>
<script>
	import Vue from 'vue'
	import {
		Header
	} from 'mint-ui';
	import {
		Button
	} from 'mint-ui';
	import {
		mapMutations
	} from 'vuex'
	import hearder from '../public/header.vue'

	export default {
		name: 'news',
		components:{
	        hearder
		},
		data() {
			return {
				is_fixed: true,
				title:'消息'
			}
		},
		methods: {

		}
	}
</script>
