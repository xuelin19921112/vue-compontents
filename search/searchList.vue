<!-- 根据OE号查询结果列表 -->
<template>
	<div v-show="show" class="search_list">
		<p class="list-title">共找到<span list-title-account>{{count}}</span>个配件</p>
		<ul class="list-items" v-infinite-scroll="loadMore" infinite-scroll-disabled="loading" infinite-scroll-immediate-check=true infinite-scroll-distance="10">
			<li v-for="item in list">
				<a @click="ToDetail(item)" class="list-item-content">
					<!--<a class="list-item-content">-->
						<img v-if="item.ContentUrl" :src="item.ContentUrl"/>
						<img v-else src="../../assets/img/search/search_nopic.png" >
						<div class="item-info">
							<p><span class="font-color">{{item.ProdBrandName}}  </span>{{item.ProdName}}</p>
							<p>{{item.Sku}}</p>
							<p><span class="font-color">{{item.DealerType==0?item.TradePrice:item.InPrice | currency}} </span>
							   <span v-if="item.DealerType==0" class="mui-badge mui-badge-success">品牌件</span>
							   <span v-if="item.DealerType==1&&item.ProdQuality" class="mui-badge mui-badge-success">{{getProdQualityLabel(item.ProdQuality)}}</span>
							</p>
						</div>
						<img src="../../assets/img/search/xcx_nextstep.png" />
					<!--</a>-->
				</a>
				<div class="business-info bdt">
					<img src="../../assets/img/search/dxj_sincerity.png" />
					<p class="business-name fl">{{formatDealerName(item.DealerName)}}</p>
					<p class="business-place fr" v-if="item.Cus_OriginPlace">产地：{{item.Cus_OriginPlace}}</p>
				</div>
			</li>
		</ul>
		  <p v-show="loading">加载中...</p>
	</div>
</template>

<script>
import { InfiniteScroll } from 'mint-ui';
import {getProdQualityLabel,formatDealerName} from '../../content/utils.js'
export default {
	name: "searchList",
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
			default:[]
		},
		count:0
	},
	data() {
		return {
		  loading:false
		}
	},
	computed: {

	},
	methods: {
		ToDetail(item){
           this.$emit('detail',item);
		},
		getProdQualityLabel(val)
		{
			return getProdQualityLabel(val);
		},
		formatDealerName(val)
		{
			return formatDealerName(val);
		},
		loadMore() {
			if(this.count==this.list.length)
			{
				return false;
			}
			this.$emit('loadnext');
			
		}
			
	}
}
</script>
