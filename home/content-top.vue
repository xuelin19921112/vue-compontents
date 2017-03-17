<!-- 首页展示采购询价数、采购订单总数、采购配件总数、云采购总金额组件 -->
<template>
	<ul class="content-top" v-show="show">
		<li class="bdb fl text-center" v-for="item in list">
			<div>
				<h3>{{item.title}}</h3>
				<span>{{item.accout}}</span>
			</div>
		</li>
	</ul>
</template>

<script>
	import {mapGetters} from 'vuex'
	export default {
		name: 'index_main',
		//首页一开始默认加载的数据
		created: function() {

		},
		props: {
			show: {
				type: Boolean,
				default: true
			},
			list:{
				type:Array,
				default:function(){
					return [];
				}
			}
		},
		data() {
			return {

			}
		},
		computed: {
			...mapGetters({
				userinfo: 'UserInfo'
			})
		},
		mounted() {

		},
		methods: {
			
		}
	}
</script>