<!-- 云销售备货单组件 -->
<template>
	<div id="stocking" class="" style="background: #fff;">
		<div id="stocking_main" v-if='showList'>
			<hearder :title="title" ></hearder>
			<div class="mian_content">
				<ul class="inquiry-titles" id="inquiry-titles">
					<li class="inquiry-title-item" v-for="item in listType" @click="query(item.index);">
						<span v-text="item.name" :class="{active:item.iscur}" @click="setActive(item)"></span>
					</li>
				</ul>
				
				<div class="inquiry-content" >
					<stocking-list  ref="prodlist" @loadnext="loadnext" :index="index" :list="list" :count="count" @showDetail="showDetail"></stocking-list>
				</div>
			</div>
		</div>
		<div id="stocking-detail" class="" v-if='showDetails'>
			<mt-header title="备货详情" :fixed="is_fixed">
				<mt-button slot="left">
					<a class="mui-icon mui-icon-left-nav mui-pull-left" @click="close"></a>
				</mt-button>
			</mt-header>
			<div class="detail-content">
				<ul class="buyer-info">
					<p style="padding-left: 0.35rem;">备货号：<span>{{details.PO_NUM}}</span></p>
					<li v-for="item in details.Po_Deatail" style="position: relative;">
						<p><span>{{item.PROD_SKU}}</span><span style="padding-left: 1.4rem;">{{item.ProductBrand_Name?item.ProductBrand_Name:''}} {{item.PROD_NAME}}</span></p>
						<p><span>单价:</span><span class="font-color-red font-small">￥</span><span class="font-color-red">{{item.IN_UNIT_PRICE}}</span></p>
						<p class="buyer-info-account">{{"x"+item.PROD_COUNT}}</p>
					</li>
				</ul>
				<ul class="detail-select">
					<li>
						<div class="mui-input-row mui-checkbox mui-left">
							<input name="checkbox" value="" type="checkbox" class="qx" v-model="details.Po_Deatail[0].IsDelivery">
							<label>送货</label>
						</div>
					</li>
					<li>
						<div class="mui-input-row mui-checkbox mui-left">
							<input name="checkbox" value="" type="checkbox" class="qx" v-model="details.Po_Deatail[0].IsTax">
							<label>含税</label>
						</div>
					</li>
					<li>
						<div class="mui-input-row mui-checkbox mui-left">
							<input name="checkbox" value="" type="checkbox" class="qx" v-model="details.Po_Deatail[0].IsPacking">
							<label>带包装</label>
						</div>
					</li>
				</ul>
				<div class="group-btn" v-if='index==1'>
					<button type="button" class="mui-btn nogoods" @click='AuditPopOrder2'>无货</button>
					<button type="button" class="mui-btn stockingbtn" @click='AuditPopOrder'>备货完成</button>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
    import { InfiniteScroll } from 'mint-ui';
	import hearder from '../public/header.vue'
	import stockingList from '../stocking/stocking-list.vue'
	import {SourcePlat} from 'src/content/status'
	import {mapActions,mapGetters} from 'vuex'
	import {Toast} from 'mint-ui';
	import api from 'src/api/urls.js'
	import {Trim} from 'src/content/utils.js';
	export default {
		name: 'stocking',
		components: {
			hearder,stockingList
		},
		data() {
			return {
				title: '备货单',
				nothingshow: true,
				listshow: true,
				listType: [{
					index: 1,
					name: '待备货',
					iscur: true
				}, {
					index: 2,
					name: '已备货',
					iscur: false
				}, {
					index: 3,
					name: '已作废',
					iscur: false
				}],
				index: 1,
				list: [],
				nowpageindex:1,
				prodlist:[],
				isqueryed:false,
				loading: false,
				wating: false,
				pagesize: 5,
				pageindex: 1,
				count: 0,
				pagetotal: 0,
				IsDelivery: false,
				IsTax: false,
				IsPacking:false,
				HasGoods: false,
				details: {},
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
		methods: {
			setActive: function(item) {
				this.listType.forEach(x => {
					x.iscur = false;
				});
				item.iscur = true;
				this.index = item.index;
			},
			showDetail(item) {
				this.details = item;
				console.log(this.details);
				this.showDetails=true;
				this.showList=false;
			},
			query(i) {
				let _this = this;
				var param = {
					Status: i,
					PageSize: this.pagesize,
					PageIndex:this.pageindex,
					DealerId: 10018, //this.userinfo.DealerId,
					DealerName: this.userinfo.DealerName,
					PopUserName: this.userinfo.UserName,
					SourcePlat: SourcePlat
				};
				this.loading = true;
				this.$store.dispatch({
					type: 'postData',
					url: api.GetPopOrderList,
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
					pageIndex:this.nowpageindex,
					DealerId: 10018, //this.userinfo.DealerId,
					DealerName: this.userinfo.DealerName,
					PopUserName: this.userinfo.UserName,
					SourcePlat: SourcePlat
				};
				this.$store.dispatch({
					type: 'postData',
					url: api.GetPopOrderList,		
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
							console.log(result);
						}
					}
				})
			},
			AuditPopOrder(){
                let _this=this;
                var param={
                    DealerId:10018, //this.userinfo.DealerId,
                   	PoNum:this.details.PO_NUM,//备货单号
                   	IsDelivery:this.details.Po_Deatail[0].IsDelivery,//是否送货
                   	IsTax:this.details.Po_Deatail[0].IsTax,//是否含税
                   	IsPacking:this.details.Po_Deatail[0].IsPacking,//是否带包装
                   	HasGoods: true//备货是否完成
                };
                this.loading=true;
                this.$store.dispatch({type : 'postData',url:api.AuditPopOrder,data:param,callBack :(data)=>{
                        _this.loading=false;
                        if(data.status=="success")
                        {
                        	Toast('操作成功返回上一级');
                        	var that = this;
                        	setTimeout(function(){
	                        	that.showDetails=false;
								that.showList=true;	
                        	},2000);
                        }
                    }
               })
			},
			AuditPopOrder2(){
                let _this=this;
                var param={
                    DealerId:10018, //this.userinfo.DealerId,
                   	PoNum:this.details.PO_NUM,//备货单号
                   	IsDelivery:this.details.Po_Deatail[0].IsDelivery,//是否送货
                   	IsTax:this.details.Po_Deatail[0].IsTax,//是否含税
                   	IsPacking:this.details.Po_Deatail[0].IsPacking,//是否带包装
                   	HasGoods: false//备货是否完成
                };
                this.loading=true;
                this.$store.dispatch({type : 'postData',url : api.AuditPopOrder,data:param,callBack :(data)=>{
                        _this.loading=false;
                        if(data.status=="success")
                        {
                        	Toast('操作成功返回上一级');
                        	var that = this;
                        	setTimeout(function(){
	                        	that.showDetails=false;
								that.showList=true;	
                        	},2000);
                        }
                    }
               })
			},
			close(){
				this.showDetails=false;
				this.showList=true;
			}
		}
	}
</script>
