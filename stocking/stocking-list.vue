<!-- 云销售备货单列表组件 -->
<template>
<!--<div class="inquiry-content" style="background: #fff;">-->
	<div class="inquiry-content-items" id="stocking-list"  >
		<ul  v-infinite-scroll="loadMore" infinite-scroll-disabled=loading  infinite-scroll-immediate-check=true infinite-scroll-distance="10">
			<!--<ul>-->
			<li class="item-content" v-for="item in list">
				<div class="title">
					<span class="buyer">备货单号：</span>
					<span class="states font-color-red" v-if='index==1'>待备货</span>
					<span class="states font-color-red" v-if='index==2'>已备货</span>
					<span class="states font-color-red" v-if='index==3'>已作废</span>
				</div>
				<div class="content">
					<ul class="content-left">
						<li v-for="i in item.Po_Deatail">
							<div class="">
								<p class="">
									<span v-text="i.PROD_SKU"></span><span style="padding-left: 1rem;">{{i.ProductBrand_Name?i.ProductBrand_Name:''}} {{i.PROD_NAME}}</span>
								</p>
								<p class="">
									<span class="font-small font-color-red">￥</span><span class="font-color-red"  v-text='i.IN_UNIT_PRICE'></span>
								</p>
							</div>
							<div class="content-right">
								<span v-text='"x"+i.PROD_COUNT'></span>
							</div>
						</li>
					</ul>

				</div>
				<div class="price">
					<span>商品金额：</span><span class="font-small font-color-red">￥</span><span class="font-color-red" v-text='item.TOTAL_PRICE'></span>
				</div>
				<div class="time" v-if="index==1">
					<span class="nowtime font-color-grey">{{item.SERVICE_DATE | dateformat('1') }}</span>
					<button type="button" class="mui-btn mui-btn-danger offer" @click="showDetail(item,1)">去备货</button>
				</div>
				<div class="time" v-if="index==2">
					<span class="nowtime font-color-grey">{{item.SERVICE_DATE | dateformat('1') }}</span>
					<button type="button" class="mui-btn mui-btn-danger detail" @click="showDetail(item,2)">详情</button>
				</div>
				<div class="time" v-if="index==3">
					<span class="nowtime font-color-grey">{{item.SERVICE_DATE | dateformat('1') }}</span>
					<button type="button" class="mui-btn mui-btn-danger detail" @click="showDetail(item,3)">详情</button>
				</div>
			</li>
		</ul>
		<p v-show="loading">加载中...</p>
	</div>
<!--</div>-->
</template>
<script>
    import { InfiniteScroll } from 'mint-ui';
	import {SourcePlat} from 'src/content/status'
	import {mapActions,mapGetters} from 'vuex'
	import {Toast} from 'mint-ui';
	import api from 'src/api/urls.js'
	import {Trim} from 'src/content/utils.js';
	export default {
		name: 'stocking-list',
		props: {
			index: {
				type: Number,
				default: true
			},
			list:{
				type:Array,
				default: []
			},
			count:{
				type:Number,
				default:0
			}
		},
		data() {
			return {
				loading:false
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
			loadMore() {
				console.log(this.loading);
				if(this.count<=0 || this.count==this.list.length)
				{
					return false;
				}
				this.$emit('loadnext',this.index);
				
			}
		}
	}
</script>