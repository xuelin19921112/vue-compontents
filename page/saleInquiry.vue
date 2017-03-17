<!-- 云销售询价单组件 -->
<template>
	<div id="saleInquiry" class="" style="background: #fff;">
		<div id="saleInquiry_main" >
			<hearder :title="title" ></hearder>
			<div class="mian_content">
				<ul class="inquiry-titles" id="inquiry-titles">
					<li class="inquiry-title-item fl bdb" v-for="item in listType" @click="query(item.index);">
						<span v-text="item.name" :class="{active:item.iscur}" @click="setActive(item)"></span>
					</li>
				</ul>
				
				<div class="inquiry-content" >
					<saleInquiry-list ref="prodlist" @loadnext="loadnext" :index="index" :list="list" :count="count"></saleInquiry-list>
				</div>
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
	import saleInquiryMain from '../saleInquiry/saleInquiry-main.vue'
	import saleInquiryList from '../saleInquiry/saleInquiry-list.vue'
	import {SourcePlat} from 'src/content/status'
	import {mapActions,mapGetters} from 'vuex'
	import {Toast} from 'mint-ui';
	import api from 'src/api/urls.js'
	import {Trim,formatDealerName,formatMoble} from '../../content/utils.js';
	
	export default {
		name: 'saleInquiry',
		components:{
	        hearder,saleInquiryMain,saleInquiryList
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
				count: 0,          
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
		computed: {
			...mapGetters({
				userinfo: 'UserInfo'
			})
		},
		mounted() {
			this.$nextTick(() => {
				this.query(1);
			})
		},
		methods : {
			setActive: function(item) {
					this.listType.forEach(x => {
						x.iscur = false;
					});
					item.iscur = true;
					this.index = item.index;
				},
			query(i) {
				let _this = this;
				var param = {
					Status: i,
					PageSize: this.pagesize,
					Page:this.pageindex,
					DealerId: 10018, //this.userinfo.DealerId,
					DealerName: this.userinfo.DealerName,
					PopUserName: this.userinfo.UserName,
					SourcePlat: SourcePlat
				};
				this.loading = true;
				this.$store.dispatch({
					type: 'postData',
					url: api.GetOfferOrderList,
					data: param,
					callBack: (data) => {
						_this.loading = false;
						if(data.status == "success") {
							let result = JSON.parse(data.result);
							_this.pagetotal = result.TotalPages;
							_this.count = result.TotalCount;
							_this.list = result.Rows;
							_this.nowpageindex = result.PageIndex;
							console.log(result);
						}
					}
				})
			},
			loadnext(i) {
				this.nowpageindex++;
				let _this = this;
				this.$refs.prodlist.loading = true;
				var param = {
					Status: i,
					Pagesize: this.pagesize,
					Page:this.nowpageindex,
					DealerId: 10018, //this.userinfo.DealerId,
					DealerName: this.userinfo.DealerName,
					PopUserName: this.userinfo.UserName,
					SourcePlat: SourcePlat
					
				};
				this.$store.dispatch({
					type: 'postData',
					url: api.GetOfferOrderList,		
					data: param,
					callBack: (data) => {
						this.$refs.prodlist.loading = false;
						if (data.status == "success") {
							let result = JSON.parse(data.result);
							_this.pagetotal = result.TotalPages;
							_this.count = result.TotalCount;
							for (let item of result.Rows) {
								_this.list.push(item);
							}
							// _this.pagecount = Math.ceil(result.count / _this.pagesize);
							console.log(result);
						}
					}
				})
			},
			AuditPopOrder(){
                let _this=this;
                var param={
                    DealerId:10018, //this.userinfo.DealerId,
                   	PopUserName:this.detail.PopUserName,//买家号
  

                };                    
                this.loading=true;         
                this.$store.dispatch({type:'postData',url:api.AuditPopOrder,data:param,callBack :(data)=>{
                        _this.loading=false;
                        if(data.status=="success")
                        {
                        	Toast('操作成功返回上一级');
                        	setTimeout(function(){
                        		this.showDetails=false;
								this.showList=true;
                        	},2000);
                        }
                    }
               })
			},
			
		}
	}
</script>
