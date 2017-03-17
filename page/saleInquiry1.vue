<!-- 云销售询价单组件 -->
<template>
	<div id="saleInquiry" class="" style="background: #fff;">
		<div id="saleInquiry_main" >
			<hearder :title="title" ></hearder>
			<div class="mian_content">
				<ul class="inquiry-titles" id="inquiry-titles">
					<li class="inquiry-title-item" v-for="item in listType" @click="query(item.index);">
						<span v-text="item.name" :class="{active:item.iscur}" @click="setActive(item)"></span>
					</li>
				</ul>
				
				<!--<div class="inquiry-content" >
					<stocking-list  ref="prodlist" @loadnext="loadnext" :index="index" :list="list" :count="count"></stocking-list>
				</div>-->
			</div>
		</div>
		
		
		
		
		<!--<!-- 顶部 -->
        <!--<hearder :title="title"></hearder>-->
        
		<!-- 页面主体 -->
        <!--<sale-inquiry-main></sale-inquiry-main>-->

	</div>
</template>

<script>
	import Vue from 'vue'
	import hearder from '../public/header.vue'
	import saleInquiryMain from '../saleInquiry/saleInquiry_main.vue'
	import {SourcePlat} from 'src/content/status'
	import {mapActions,mapGetters} from 'vuex'
	import {Toast} from 'mint-ui';
	import api from 'src/api/urls.js'
	import {Trim} from 'src/content/utils.js';
	
	export default {
		name: 'saleInquiry',
		components:{
	        hearder,saleInquiryMain
		},
		data () {
			return {
				title:'询价单',
				nothingshow: true,
				listshow: true,
				listType: [{
					index: 1,
					name: '待报价',
					iscur: true
				}, 
				{
					index: 2,
					name: '已报价',
					iscur: false
				}, 
				{
					index: 3,
					name: '已过期',
					iscur: false
				}],		
				index: 1,
				list: [],
				nowpageindex:1,
				prodlist:[],
				isqueryed:false,  //正在查询	
				loading: false,    //加载
				wating: false,     //等待
				pagesize: 5,       //一次请求5条
				pageindex: 1,      //后台接口第一页
				count: 0,          // 
				pagetotal: 0,
				IsDelivery: false,    
				IsTax: false,
				IsPacking:false,
				HasGoods: false,
				status: 1,
				detail: {},
				showDetails:false,
				showList:true
			}
		},
		methods : {
			setActive: function(item) {
					this.listType.forEach(x => {
						x.iscur = false;
					});
					item.iscur = true;
					this.index = item.index;
				},
			
		}
	}
</script>
