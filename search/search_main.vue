<!-- 找配件主体组件 -->
<template>
	<div id="search_main">
		<div v-show="!showgoodsinfo&&!showcarmodel">
			<!-- 顶部 -->
			<hearder :title="title"></hearder>
			<div class="mian_content">
				<ul class="search-titles bdb" id="search-titles">
					<li class="search-title-item fl text-center" v-for="item in typelist">
						<span v-text="item.name" :class="{active:item.iscur}" @click="setActive(item)"></span>
					</li>
					<img src="../../assets/img/search/line.png" class="search-title-division" />
				</ul>
				<div class="search-content text-center">
					<div class="search-content-items" v-if="index==0">
						<textarea class="search-oe bd" v-model="skus" placeholder='请输入OE号，一次最多查询10个OE号，多个OE号用","隔开'></textarea>
						<button class="search-oe-btn btn-grey" :class="{'active-btn-green':skus!=''}" @click="query">{{querying?'查询中':'查询'}}</button>
					</div>
					<div class="search-content-items" v-if="index == 1">
						<div @click="showcarmodel=true" class="search-car bdb">
							<img src="../../assets/img/search/zpj_choose_car.png" class="img-center" />
							<span class="overflow">{{carmodelinfo?carmodelinfo.AutoYear:"请先选择车型以便于精准查找配件"}}</span>
							<img src="../../assets/img/search/xcx_nextstep.png" class="img-center" />
						</div>
						<!--<ul class="mui-table-view mui-table-view-chevron">
							<li class="mui-table-view-cell mui-media">
									<a class="mui-navigate-right" href="#">
										<img v-if="!carmodelinfo" class="mui-media-object mui-pull-left" src="../../assets/img/search/zpj_choose_car.png">
										<img v-if="carmodelinfo" class="mui-media-object mui-pull-left" :src="carmodelinfo.brandicon">
										<div class="mui-media-body">
											<p  class='mui-ellipsis'>{{carmodelinfo?carmodelinfo.name:'请先选择车型以便于精准查找配件'}}</p>
										</div>
									</a>
							</li>
						</ul>-->
						<div class="search-box bd">
							<input v-model.trim="prodname" type="search" placeholder="活塞花" />
							<img src="../../assets/img/search/xcx_search.png" id="search-parts-btn" @click="querybyname()" />
						</div>
						<!--输入配件名称，查询到没有结果时展示-->
						<!--<seach-nothing v-show="nothingshow"></seach-nothing>-->

						<!--输入配件名称，查询到结果时展示-->
						<!--<search-list@loadnext="loadnext" v-show="isqueryed&&prodlist.length" :list="prodlist" :count="count"></search-list>-->
					</div>

					<!--输入OE号，查询到没有结果时展示-->
					<seach-nothing v-show="isqueryed&&!prodlist.length&&!querying"></seach-nothing>

					<!--输入OE号，查询到结果时展示-->
					<search-list ref="prodlist" @detail="togoodsdetail" @loadnext="loadnext" v-show="isqueryed&&prodlist.length" :list="prodlist"
						:count="count"></search-list>
				</div>
			</div>
		</div>
		<goodsInfo v-model="showgoodsinfo" :prodInfo="prodInfo"></goodsInfo>
        <car-model v-model="showcarmodel" ></car-model>
	</div>
</template>

<script>
    import eventHub from 'src/bus';
   	import hearder from '../public/header.vue';
    import goodsInfo from 'components/goodsInfo/goodsInfo_main.vue';
	import carModel from 'components/car/car.vue';
	import { mapGetters } from 'vuex';
	import { Toast } from 'mint-ui';
	import { Trim } from 'src/content/utils.js';
	import seachNothing from './searchNothing.vue';
	import searchList from './searchList.vue';
	import api from '../../api/urls.js'
	import {SourcePlat} from 'src/content/status'
	export default {
		name: 'search_main',
		//首页一开始默认加载的数据
		created: function () {
			let _this=this;
			eventHub.$on('goodsinfo',function(value){
				_this.showgoodsinfo=value;
			})
		},
		beforeDestroy(){
			eventHub.$off('goodsinfo')
		},
		components: {
			seachNothing, searchList,goodsInfo,hearder,carModel
		},
		data() {
			return {
				typelist: [
					{
						index: 0,
						name: 'OE号查询',
						iscur: true,
						oe: true
					},
					{
						index: 1,
						name: '配件查询',
						iscur: false,
						oe: false
					}
				],
				querying: false,
				prodname:"",
				prodlist: [],
				prodInfo:{},
				scrolldistance:0,
				showgoodsinfo:false,
				showcarmodel:false,
				isqueryed: false,
				select: false,
				count: 0,
				pagesize: 10,
				pageindex: 1,
				pagecount: 0,
				skus: "",
				index: 0,
				title:'找配件'
			}
		},
		computed: {
			searchsku(){
				 return this.skus.replace(/[&\|\\\*^%$#@\-]/g,"").replace(/，/g,",");
			},
			...mapGetters({
				userinfo:'UserInfo',
				carmodelinfo:'CarModelInfo'
			})
		},
		watch:{
           showgoodsinfo(val){
			  if(!val&&this.scrolldistance){
                 $("html,body").animate({scrollTop:this.scrolldistance},100);
			  }
		   }
		},
		methods: {
			querybyname(){
               if(!this.carmodelinfo){
				   Toast('请选择车型')
				   return false;
			   }
			   if(!this.prodname){
                   Toast('请输入配件名称')
				   return false;
			   }
                this.pageindex = 1;
				this.prodlist = [];
				let _this = this;
				this.count = 0;
				this.pagecount = 0;
				_this.querying = true;
			   let param={ IsRecord:true, SourcePlat:SourcePlat, DealerId: this.userinfo.DealerId, pageindex: this.pageindex, pagesize: this.pagesize,IsAdvancedSearch: true, SearchType: 2 };
			   param.FactoryId=this.carmodelinfo.FactoryId;
			   param.CarModelId=this.carmodelinfo.CarModelId;
			   param.CarYearId=this.carmodelinfo.AutoYearId;
			   param.Vin=this.carmodelinfo.Vin;
			   param.ProdName=this.prodname;
				//查找配件（根据车型 名称）
				this.$store.dispatch({
					type: 'postData',
					url: api.SearchPopProducts,
					data: param,
					callBack: (data) => {
						if (data.status == "success") {
							_this.isqueryed = true;
							_this.querying = false;
							let result = JSON.parse(data.result);
							result.list.forEach(x => {
								x.quantity = 1
							})
							_this.prodlist = result.list;
							_this.count = result.count;
							_this.pagecount = Math.ceil(result.count / _this.pagesize);
						}
					}
				})
			},
			query() {
				if (this.skus == "") {
					return false;
				}
				if(this.searchsku.split(',').length>10){
					Toast('OE号不能超过十个');
					return false;
				}
				this.pageindex = 1;
				this.prodlist = [];
				let _this = this;
				this.count = 0;
				this.pagecount = 0;
				_this.querying = true;
				//查找配件(根据oe号)
				this.$store.dispatch({
					type: 'postData',
					url: api.SearchPopProducts,
					data: { IsRecord:true, SourcePlat:SourcePlat, DealerId: this.userinfo.DealerId, pageindex: this.pageindex, pagesize: this.pagesize, Sku: this.skus, IsAdvancedSearch: true, SearchType: 1 },
					callBack: (data) => {
						if (data.status == "success") {
							_this.isqueryed = true;
							_this.querying = false;
							let result = JSON.parse(data.result);
							result.list.forEach(x => {
								x.quantity = 1
							})
							_this.prodlist = result.list;
							_this.count = result.count;
							_this.pagecount = Math.ceil(result.count / _this.pagesize);
						}
					}
				})
			},
			togoodsdetail(item) {
               this.prodInfo=item;
			   this.showgoodsinfo=true;
			   this.scrolldistance=document.body.scrollTop;
			},
			loadnext() {
				this.pageindex++;
				let _this = this;
				this.$refs.prodlist.loading = true;

				if(this.index==1){
					let param={ IsRecord:true, SourcePlat:SourcePlat, DealerId: this.userinfo.DealerId, pageindex: this.pageindex, pagesize: this.pagesize,IsAdvancedSearch: true, SearchType: 2 };
					param.FactoryId=this.carmodelinfo.FactoryId;
					param.CarModelId=this.carmodelinfo.CarModelId;
					param.CarYearId=this.carmodelinfo.AutoYearId;
					param.Vin=this.carmodelinfo.Vin;
					param.ProdName=this.prodname;
					//查找配件（根据车型 名称）
					this.$store.dispatch({
						type: 'postData',
						url: api.SearchPopProducts,
						data: param,
						callBack: (data) => {
							this.$refs.prodlist.loading = false;
							if (data.status == "success") {
								let result = JSON.parse(data.result);
								for (let item of result.list) {
									item.quantity = 1;
									_this.prodlist.push(item);
								}
								_this.count = result.count;
								_this.pagecount = Math.ceil(result.count / _this.pagesize);
							}
						}
					})
				}else{
					//获取数据demo
					this.$store.dispatch({
						type: 'postData',
						url: api.SearchPopProducts,
						data: { DealerId: 10385, pageindex: this.pageindex, pagesize: this.pagesize, Sku: this.skus, IsAdvancedSearch: true, SearchType: 1 },
						callBack: (data) => {
							this.$refs.prodlist.loading = false;
							if (data.status == "success") {
								let result = JSON.parse(data.result);
								for (let item of result.list) {
									item.quantity = 1;
									_this.prodlist.push(item);
								}
								_this.count = result.count;
								_this.pagecount = Math.ceil(result.count / _this.pagesize);
							}
						}
					})
				}

			},
			setActive: function (item) {
                this.pageindex = 1;
				this.prodlist = [];
				this.count = 0;
				this.pagecount = 0;
				this.skus="";
				this.prodname="";
                this.isqueryed=false;
				this.typelist.forEach(x => {
					x.iscur = false;
				});
				item.iscur = true;
				this.index = item.index;
				console.log(item.index);
			},
			tap: function () {
				if (this.select == false) {
					return mui.toast('为了更精准的匹配配件，请先选择车型');
				}
			}
		}
	}

</script>
