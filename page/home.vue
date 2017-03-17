<!-- 云采购主页组件 -->
<template>
	<div id="home" class="app">
		<!-- 顶部 -->
        <hearder :title="title"></hearder>
        
		<!-- 页面主体 -->
        <div id="main" class="">
			<div class="mian_content">
				<content-top  :list="contentTop"></content-top>
				<ul class="content-main">
	    			<li class="fl text-center">
	    				<router-link to="/search">
	    					<div class="content-main-name">
		    					<img src="../../assets/img/home/cloud_procurement_pj.png" />
		    					<span>找配件</span>
		    				</div>
	    				</router-link>
	    			</li>
	    			<li class="fl text-center">
	    				<router-link to="/pendingInquiry">
	    					<div class="content-main-name">
		    					<img src="../../assets/img/home/cloud_procurement_dxj.png" />
		    					<span>待询价</span>
		    					<span v-show="maindata[0]" class="mui-badge mui-badge-danger content-main-pos">{{maindata[0]}}</span>
		    				</div>
	    				</router-link>
	    			</li>
	    			<li class="fl text-center">
	    				<router-link to="/pubInquiry">
	    					<div class="content-main-name">
		    					<img src="../../assets/img/home/cloud_procurement_xj.png" />
		    					<span>询价单</span>
		    					<span v-show="maindata[1]"  class="mui-badge mui-badge-danger content-main-pos">{{maindata[1]}}</span>
		    				</div>
	    				</router-link>
	    			</li>
	    			<li class="fl text-center">
	    				<!--<router-link to="#">-->
	    					<div class="content-main-name">
		    					<img src="../../assets/img/home/cloud_procurement_cg.png" />
		    					<span>采购订单</span>
		    					<span v-show="maindata[2]"  class="mui-badge mui-badge-danger content-main-pos">{{maindata[2]}}</span>
		    				</div>
	    				<!--</router-link>-->
	    			</li>
	    			<li class="fl text-center">
	    				<!--<router-link to="#">-->
	    					<div class="content-main-name">
		    					<img src="../../assets/img/home/cloud_procurement_sh.png" />
		    					<span>售后服务</span>
		    				</div>
	    				<!--</router-link>-->
	    			</li>
	    			<li class="fl text-center">
	    				<router-link to="/login">
	    					<div class="content-main-name" >
		    					<img src="../../assets/img/home/cloud_procurement_dz.png" />
		    					<span>地址管理</span>
		    				</div>
	    				</router-link>
	    			</li>
				</ul>
			</div>
	</div>
        
		<!-- 底部导航栏 -->
        <homefooter></homefooter>
	</div>
</template>

<script>
import hearder from '../public/header.vue'
import contentTop from '../home/content-top.vue'
import homefooter from '../home/index_footer.vue'
import { mapGetters } from 'vuex'
import api from '../../api/urls.js'
import {MD5,Trim,CheckUserName,getcurrDateTime} from '../../content/utils.js';
import {SourcePlat,ProdQuality} from 'src/content/status'
export default {
	name: 'home',
	components:{
        hearder,contentTop,homefooter
	},
	data () {
		return {
			homeData:[0,0,0,0],
			contentTop:[{
				title:'采购询价数',
				accout:0
			},{
				title:'采购订单总数',
				accout:0
			},{
				title:'采购配件总数',
				accout:0
			},{
				title:'云采购总金额',
				accout:0
			}],
			title:'云采购',
			maindata:[0,0,0] //待询价配件数 询价单个数 订单个数
		}
	},
	computed : {
        ...mapGetters(
			{userinfo:'UserInfo'}
		)
	},
	mounted(){
        this.$nextTick(()=>{
              this.GetInquiry();
		})
	},
	methods : {
		GetInquiry(){
			let _this=this;
			var param={
				SourcePlat:SourcePlat,
				DealerId:this.userinfo.DealerId,
				DealerName:this.userinfo.DealerName,
				PopUserName:this.userinfo.UserName
			};
			this.$store.dispatch({type : 'postData',url : api.GetInquiryCount,data:param,callBack :(data)=>{
					if(data.status=="success")
					{
						let result=JSON.parse(data.result);
						this.maindata=[result.productCount,result.inquiryOrderCount,result.orderCount]
					}
				}
			})
			this.$store.dispatch({type : 'postData',url : api.GetPurchaseSummary,data:param,callBack :(data)=>{

					if(data.status=="success")
					{
						let result=JSON.parse(data.result);
						this.contentTop[0].accout=result.InquiryOrderCount;
						this.contentTop[1].accout=result.OrderCount;
						this.contentTop[2].accout=result.ProductCount;
						this.contentTop[3].accout=result.OrderTotal;
						// this.maindata=[result.productCount,result.inquiryOrderCount,result.orderCount]
					}
				}
			})
		}
	}
}
</script>


