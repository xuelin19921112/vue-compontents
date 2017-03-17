<!-- 云销售主页组件 -->
<template>
	<div id="sale" class="app">
		<!-- 顶部 -->
        <hearder :title="title"></hearder>
        
		<!-- 页面主体 -->
        <div id="main" class="">
			<div class="mian_content">
				<content-top  :list="contentTop"></content-top>
				<ul class="content-main sale-content">
	    			<li class="fl text-center">
	    				<router-link to="/saleInquiry">
	    					<div class="content-main-name">
		    					<img src="../../assets/img/sale/cloud_procurement_xj.png" />
		    					<span>询价单</span>
		    					<span v-show="saleMainData[0]" class="mui-badge mui-badge-danger content-main-pos">{{saleMainData[0]}}</span>
		    				</div>
	    				</router-link>
	    			</li>
	    			<li class="fl text-center">
	    				<router-link to="/stocking">
	    					<div class="content-main-name">
		    					<img src="../../assets/img/sale/cloud_procurement_bhd.png" />
		    					<span>备货单</span>
		    					<span v-show="saleMainData[1]" class="mui-badge mui-badge-danger content-main-pos">{{saleMainData[1]}}</span>
		    				</div>
	    				</router-link>
	    			</li>
	    			<li class="fl text-center">
	    				<router-link to="/car">
	    					<div class="content-main-name">
		    					<img src="../../assets/img/sale/cloud_procurement_xsdd.png" />
		    					<span>销售订单</span>
		    					<span v-show="saleMainData[2]" class="mui-badge mui-badge-danger content-main-pos">{{saleMainData[2]}}</span>
		    				</div>
	    				</router-link>
	    			</li>
	    			<li class="fl text-center">
	    				<!--<router-link to="#">-->
	    					<div class="content-main-name">
		    					<img src="../../assets/img/sale/cloud_procurement_th.png" />
		    					<span>退货管理</span>
		    				</div>
	    				<!--</router-link>-->
	    			</li>
				</ul>
			</div>
	</div>
        
		<!-- 底部导航栏 -->
        <salefooter></salefooter>
	</div>
</template>

<script>
import hearder from '../public/header.vue'
import contentTop from '../home/content-top.vue'
import salefooter from '../sale/sale_footer.vue'
import { mapGetters } from 'vuex'
import api from '../../api/urls.js'
import {MD5,Trim,CheckUserName,getcurrDateTime} from '../../content/utils.js';
import {SourcePlat,ProdQuality} from 'src/content/status'
export default {
	name: 'sale',
	components:{
        hearder,contentTop,salefooter
	},
	data () {
		return {
			homeData:[0,0,0,0],
			contentTop:[{
				title:'标准化配件数',
				accout:0
			},{
				title:'询价单总数',
				accout:0
			},{
				title:'云销售订单总数',
				accout:0
			},{
				title:'云销售总金额',
				accout:0
			}],
			title:'云销售',
			saleData:[0,0,0,0], //配件数、询价单数、订单总数、总金额
			saleMainData:[0,0,0] //询价单数、备货单数、销售单数
		}
	},
	computed : {
        ...mapGetters(
			{userinfo:'UserInfo'}
		)
	},
	mounted(){
        this.$nextTick(()=>{
              this.GetSeller();
		})
	},
	methods : {
		GetSeller(){
			let _this=this;
			var param={
				SourcePlat:SourcePlat,
				DealerId:this.userinfo.DealerId,
				DealerName:this.userinfo.DealerName,
				PopUserName:this.userinfo.UserName
			};
			this.$store.dispatch({type : 'postData',url : api.GetSellerOrderCount,data:param,callBack :(data)=>{
					_this.logining=false;
					if(data.status=="success")
					{
						let result=JSON.parse(data.result);
						console.log(result);
						this.saleMainData=[result.inquiryOrderCount,0,result.orderCount]
					}
				}
			})
			this.$store.dispatch({type : 'postData',url : api.GetSellSummary,data:param,callBack :(data)=>{

					if(data.status=="success")
					{
						let result=JSON.parse(data.result);
						this.contentTop[0].accout=result.ProductCount;
						this.contentTop[1].accout=result.InquiryOrderCount;
						this.contentTop[2].accout=result.OrderCount;
						this.contentTop[3].accout=result.OrderTotal;
						// this.maindata=[result.productCount,result.inquiryOrderCount,result.orderCount]
					}
				}
			})
		}
	}
}
</script>
