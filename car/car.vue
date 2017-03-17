<!-- 选车型主体组件 -->
<template>
	<div v-show="value">

		<!-- 顶部 -->
		<div v-show="step==0" id="car_header" class="">
			<mt-header title="选车型" :fixed="is_fixed">
				<mt-button slot="left">
					<a @click="close" class=" mui-icon mui-icon-left-nav mui-pull-left"></a>
				</mt-button>
				<mt-button slot="right">
					<img src="../../assets/img/carchoose/xcx_history.png" alt="" width="20" height="20">
				</mt-button>
			</mt-header>
			<div class="car-search">
				<input type="search" v-model="vincode" class="car-VIN" placeholder="请输入17位VIN码查询车型">
				<img @click="vinsearch" src="../../assets/img/search/xcx_search.png" class="xcx_search" />
			</div>

		<!--VIN查询无结果-->
		<div v-if="Isvinsearched&&!years.length" class="car-nVIN-result">
			<img src="../../assets/img/search/xcx_car_no.png">
			<p>当前没有查询到相关车型信息</p>
			<p>请检查VIN是否正确或使用品牌选车</p>
		</div>
		
		<!--VIN查询结果列表-->
		<div v-show="years.length" class="car-hVIN-result">
			<ul class="mui-table-view">
    			<li @click="setcarmodel(item)" v-for="item in years" class="mui-table-view-cell">
        			<a class="mui-navigate-right">{{item.AutoYear}}</a>
    			</li>
			</ul>
		</div>        

		</div>

		<div v-show="showcarmodel">
			<!--选车型首页-->
			<div v-show="!carInfo.brandId" id="car_main">

				<div class="mian_content">
					<mt-index-list :height="600">
						<mt-index-section v-for="item in list" :index="item.firCode">
							<li v-for="m in item.items" @click="GetCarModel(m)">
							 <mt-cell :title="m.name">
								  <img slot="icon" :src="m.icon" style="width: 1.8rem;height: 1.35rem;">
							 </mt-cell>
							</li>
						</mt-index-section>
					</mt-index-list>
				</div>
			</div>

			<!--选择车系-->
			<div v-show="carInfo.brandId&&!carInfo.CarModelId" id="car_series" class="car-series-content">
				<mt-header title="请选择车系" :fixed="true" class="car-title car-line">
					<mt-button slot="left">
						<a @click="clearstep(0)" class=" mui-icon mui-icon-left-nav mui-pull-left"></a>
					</mt-button>
				</mt-header>
				<div class="car-lb">
					<img class="car-logo" :src="carInfo.brandicon">
					<span class="car-band">{{carInfo.brandName}}</span>
				</div>
				<div v-for="item in carmodellist" class="car-series-name">
					<p class="car-cata">{{item.name}}</p>
					<ul class="mui-table-view car-series-name">
						<li v-for="m in item.items" @click="GetYears(item,m)" class="mui-table-view-cell">
							<a class="mui-navigate-right">{{m.name}}</a>
						</li>
					</ul>
				</div>
			</div>

			<!--选择年款 大年款-->
			<div v-show="step==2" id="car_byear" class="">
				<mt-header title="请选择年款" :fixed="true">
					<mt-button slot="left">
						<a @click="clearstep(1)" class="mui-icon mui-icon-left-nav mui-pull-left"></a>
					</mt-button>
				</mt-header>
				<div class="byear-content">
					<p class="byear-content-title">
						<span class="font-color-green"><a v-if="carInfo.brandName">{{carInfo.brandName}}</a> > <a v-if="carInfo.CarModelName">{{carInfo.CarModelName}}</a> > <a v-if="carInfo.YearName">{{carInfo.YearName}}</a></span>
					</p>
					<ul class="mui-table-view byear-content-items">
						<li v-for="item in yearlist " @click="GetSubYears(item)" class="mui-table-view-cell">
							<!--<router-link to="/carFconfig">-->
							<a class="mui-navigate-right">{{item.name}}</a>
							<!--</router-link>-->
						</li>
					</ul>
				</div>
			</div>

			<!--选择年款 小年款-->
			<div v-show="step==3" id="car_syear" class="">
				<mt-header title="请选择年款" :fixed="true">
					<mt-button slot="left">
						<a @click="clearstep(2)" class="mui-icon mui-icon-left-nav mui-pull-left"></a>
					</mt-button>
				</mt-header>
				<div class="byear-content">
					<p class="byear-content-title">
						<span class="font-color-green"><a v-if="carInfo.brandName">{{carInfo.brandName}}</a> > <a v-if="carInfo.CarModelName">{{carInfo.CarModelName}}</a> > <a v-if="carInfo.YearName">{{carInfo.YearName}}</a></span>
					</p>
					<ul class="mui-table-view byear-content-items">
						<li v-for="item in subyearlist " @click="GetCarconfig(item.name)" class="mui-table-view-cell">
							<!--<router-link to="/carFconfig">-->
							<a class="mui-navigate-right">{{item.name}}</a>
							<!--</router-link>-->
						</li>
					</ul>
				</div>
			</div>


			<!--选择配置 第一步-->
			<div v-show="step==4" id="car_fconfig" class="">
				<mt-header title="请选择配置" :fixed="is_fixed">
					<mt-button slot="left">
						<a @click="carback()" class=" mui-icon mui-icon-left-nav mui-pull-left"></a>
					</mt-button>
				</mt-header>
				<div class="fconfig-content">
					<p class="fconfig-content-title">
						<span class="font-color-green">
						<a v-if="carInfo.brandName">{{carInfo.brandName}}</a> > <a v-if="carInfo.CarModelName">{{carInfo.CarModelName}}</a> > <a v-if="carInfo.YearName">{{carInfo.YearName}}</a>
					    <a v-if="carInfo.SubYearName">>{{carInfo.SubYearName}}</a>
					</span>
					</p>
					<ul class="mui-table-view fconfig-content-items">
						<li v-for="item in carconfiglist " @click="GetCarLastStep(item)" class="mui-table-view-cell">
							<!--<router-link to="/carFconfig">-->
							<a class="mui-navigate-right">{{item.name}}</a>
							<!--</router-link>-->
						</li>
					</ul>
				</div>
			</div>

			<!--选择配置 第二步-->
			<div v-show="step==5" id="car_sconfig" class="">
				<mt-header title="请选择配置" :fixed="is_fixed">
					<mt-button slot="left">
						<a @click="clearstep(4)" class=" mui-icon mui-icon-left-nav mui-pull-left"></a>
					</mt-button>
				</mt-header>
				<div class="sconfig-content">
					<p class="sconfig-content-title">
						<span class="font-color-green">
						<a v-if="carInfo.brandName">{{carInfo.brandName}}</a> > <a v-if="carInfo.CarModelName">{{carInfo.CarModelName}}</a> > <a v-if="carInfo.YearName">{{carInfo.YearName}}</a>
					    <a v-if="carInfo.SubYearName">>{{carInfo.SubYearName}}</a><a v-if="carInfo.stepName">>{{carInfo.stepName}}</a>
					</span>
					</p>
					<ul class="mui-table-view fconfig-content-items">
						<li v-for="item in carlastlist " @click="carcomplete(item)" class="mui-table-view-cell">
							<!--<router-link to="/carFconfig">-->
							<a class="mui-navigate-right">{{item.name}}</a>
							<!--</router-link>-->
						</li>
					</ul>
				</div>
			</div>
		</div>


	</div>
</template>

<script>
	import { IndexList,Toast } from 'mint-ui';
	import { mapActions, mapGetters } from 'vuex'
	import api from '../../api/urls.js'
	import { MD5, Trim, CheckUserName, getcurrDateTime ,IsVINCode } from '../../content/utils.js';
	export default {
		name: 'car',
		components: {
		},
		props: {
			value: {
				type: Boolean,
				default: false
			}
		},
		data() {
			return {
				vincode:"",
				Isvinsearched:false,
				years:[],
				showcarmodel:true,
				list: [],
				carmodellist: [],
				yearlist: [],
				subyearlist: [],
				carconfiglist: [],
				carlastlist: [],
				step: 0,
				carInfo: { name: "", brandId: 0, brandName: "", brandicon: "", FacId: 0, FacName: "", CarModelId: 0, CarModelName: "", otherParam: "", YearId: 0, YearName: "", SubYearName: "", stepId:0, stepName: "",lastId:0, lastName: "" }, //2奔驰 3宝马
				carModelInfo:{CarName:"",FactoryId:0,FactoryName:"",CarModelId:0,CarModelName:"",AutoYearId:"",AutoYear:"",CarIcon:"",Vin:""},
				IsBMW: false,
				IsBenz: false
			}
		},
		computed: {

		},
		watch: {
			vincode(val){
               if(val=="")
			   {
				   this.showcarmodel=true;
			   }else{
				   this.showcarmodel=false;
			   }
			},
			value(val) {
				if (val) {
					this.vincode="";
					this.carInfo = { name: "", brandId: 0, brandName: "", brandicon: "", FacId: 0, FacName: "", CarModelId: 0, CarModelName: "", otherParam: "", YearId: 0, YearName: "", SubYearName: "", stepName: "", lastName: "" };//2奔驰 3宝马
					this.step = 0;
					this.carmodellist = [];
					this.yearlist = [];
					this.subyearlist = [];
					this.carconfiglist = [];
					this.carlastlist = [];
				}
			}
		},
		mounted() {
			this.$nextTick(() => {
				this.query();
			})
		},
		methods: {
			vinsearch(){
				var code=Trim(this.vincode);
				var msg=IsVINCode(code);
				if(msg)
				{
					Toast(msg);
					return false;
				}
				let _this=this;
				this.$store.dispatch({type : 'postData',url : api.GetCarListByVinCode,data:{Vin:code},callBack :(data)=>{
						if(data.status=="success")
						{
							let result=JSON.parse(data.result);
							if(result.length==1){
								this.setcarmodel(result[0])
							}
							this.years=result;
						}
					}
				})

			},
			setcarmodel(item){
				this.carModelInfo.AutoYearId=item.AutoYearId;
                this.carModelInfo.AutoYear=item.AutoYear;
				this.carModelInfo.CarModelId=item.CarSeriesId;
				this.carModelInfo.CarModelName=item.CarSeries;
                this.carModelInfo.FactoryId=item.FactoryId;
				this.carModelInfo.FactoryName=item.FactoryName;
                this.carModelInfo.Vin=Trim(this.vincode);
				this.addCarModelInfo(JSON.parse(JSON.stringify(this.carModelInfo)));
				console.log(JSON.parse(JSON.stringify(this.this.carModelInfo)));
				this.$emit('input', false);
			},
			GetCarModel(item) { //获取主机厂 车型
				this.step = 1;
				this.carInfo.brandId = item.id;
				if (this.carInfo.brandId == 2) {
					this.IsBenz = true;
				} else {
					this.IsBenz = false;
				}
				if (this.carInfo.brandId == 3) {
					this.IsBMW = true;
				} else {
					this.IsBMW = false;
				}

				this.carInfo.brandName = item.name;
				this.carInfo.brandicon = item.icon;
				// console.log(JSON.parse(JSON.stringify(item)) );
				this.$store.dispatch({
					type: 'postJavaData', method: api.GetCarModel, data: { brandId: this.carInfo.brandId, step: 1 }, callBack: (res) => {
						res = JSON.parse(res);
						if (res.msg == "OK") {
							this.carmodellist = res.result;
						}
					}
				})
			},
			GetYears(pitem, item) { //获取大年款
				this.step = 2;
				this.carInfo.FacId = pitem.id;
				this.carInfo.FacName = pitem.name;
				this.carInfo.CarModelId = item.id;
				this.carInfo.otherParam = item.otherParam;
				this.carInfo.CarModelName = item.name;
				this.$store.dispatch({
					type: 'postJavaData', method: api.GetCarModel, data: { brandId: this.carInfo.brandId, carModelId: this.carInfo.CarModelId, otherParam: this.carInfo.otherParam, step: 2 }, callBack: (res) => {
						res = JSON.parse(res);
						if (res.msg == "OK") {
							this.yearlist = res.result;
						}
					}
				})
			},
			GetSubYears(item) { //获取小年款
				this.step = 3;
				this.carInfo.YearName = item.name;
				this.$store.dispatch({
					type: 'postJavaData', method: api.GetCarModel, data: { brandId: this.carInfo.brandId, carModelId: this.carInfo.CarModelId, otherParam: this.carInfo.otherParam, step: 3, years: this.carInfo.YearName }, callBack: (res) => {
						res = JSON.parse(res);
						if (res.msg == "OK") {
							this.subyearlist = res.result;
							if (!this.subyearlist.length) {
								this.GetCarconfig();
							}
						}
					}
				})
			},
			GetCarconfig(subyearname) {
				this.step = 4;
				if (subyearname) {
					this.carInfo.SubYearName = subyearname;
				}

				let param = {
					brandId: this.carInfo.brandId,
					carModelId: this.carInfo.CarModelId,
					otherParam: this.carInfo.otherParam,
					step: 4,
					subYears: this.carInfo.SubYearName,
					years: this.carInfo.YearName
				}
				this.$store.dispatch({
					type: 'postJavaData', method: api.GetCarModel, data: param, callBack: (res) => {
						res = JSON.parse(res);
						if (res.msg == "OK") {
							// if (this.IsBMW || this.IsBenz) {
							this.carconfiglist = res.result;
							// } else {
							// 	this.carlastlist = res.result;
							// }


						}
					}
				})
			},
			GetCarLastStep(item) {
	            if (item) {
					this.carInfo.stepId = item.id;
					this.carInfo.stepName = item.name;
				}
				//判断跳转成功还是继续下一步
				if (!this.IsBMW && !this.IsBenz) {
					this.carcomplete();
					return false;
				}
				this.step = 5;
			
				let param = {
					brandId: this.carInfo.brandId,
					carModelId: this.carInfo.CarModelId,
					otherParam: this.carInfo.otherParam,
					step: 5,
					step2: this.carInfo.stepName,
					subYears: this.carInfo.SubYearName,
					years: this.carInfo.YearName
				}

				this.$store.dispatch({
					type: 'postJavaData', method: api.GetCarModel, data: param, callBack: (res) => {
						res = JSON.parse(res);
						if (res.msg == "OK") {
							this.carlastlist = res.result;

						}
					}
				})
			},
			carcomplete(item) {
				if (item) {
					this.carInfo.lastId = item.id;
					this.carInfo.lastName = item.name;
				}
				
				// FacId: 0, FacName: "", CarModelId: 0, CarModelName: "", otherParam: "", YearId: 0, YearName: "", 
				//{CarName:"",FactoryId:0,FactoryName:"",CarModelId:0,CarModelName:"",CarYearId:"",CarYearName:"",CarIcon:"",Vin:""},
                this.carModelInfo.AutoYear=this.carInfo.FacName + this.carInfo.CarModelName + this.carInfo.YearName;
                this.carModelInfo.FactoryId=this.carInfo.FacId;
				this.carModelInfo.FactoryName=this.carInfo.FacName;
				this.carModelInfo.CarModelId=this.carInfo.CarModelId;
				this.carModelInfo.CarModelName=this.carInfo.CarModelName;

				this.carModelInfo.AutoYearId=this.carInfo.lastId?this.carInfo.lastId:this.carInfo.stepId;
				this.carModelInfo.CarIcon=this.carInfo.brandicon;
				this.carModelInfo.Vin="";
				this.addCarModelInfo(JSON.parse(JSON.stringify(this.carModelInfo)));
				console.log(JSON.parse(JSON.stringify(this.carModelInfo)));
				this.$emit('input', false);
			},
			query() {
				this.$store.dispatch({
					type: 'postJavaData', method: api.GetCarModel, data: { step: 0 }, callBack: (res) => {
						res = JSON.parse(res);
						if (res.msg == "OK") {
							let result = res.result;
							let firs = new Set();
							let codes = [];
							res.result.map(x => firs.add(x.firCode));
							firs.forEach(x => codes.push(x));
							codes.sort();
							let arr = [];
							for (let f of codes) {
								var items = res.result.filter(k => {
									return k.firCode == f;
								})
								var obj = { firCode: f, items: items };
								arr.push(obj);
							}
							this.list = arr;
						}
					}
				})
			},
			carback() {
				if (this.carInfo.SubYearName) {
					this.clearstep(3) //go=>小年款
					return false;
				}
				if (this.carInfo.YearName) {
					this.clearstep(2) //go=>大年款
					return false;
				}
			},
			clearstep(val) {
				if (val == 0) { //go=>选车首页
					this.step = 0;
					this.carInfo = { name: "", brandId: 0, brandName: "", brandicon: "", FacId: 0, FacName: "", CarModelId: 0, CarModelName: "", otherParam: "", YearId: 0, YearName: "", SubYearId: 0, SubYearName: "",stepId:0, stepName: "",lastId:0, lastName: "" }
					this.IsBMW = false;
					this.Benz = false;
					this.carmodellist = [];
					this.yearlist = [];
					this.subyearlist = [];
					this.carconfiglist = [];
					this.carlastlist = [];
					this.step = 0;
				}
				if (val == 1) { // go=>选车系
					this.step = 1;
					this.carInfo.FacId = 0;
					this.carInfo.FacName = "";
					this.carInfo.CarModelId = 0;
					this.carInfo.CarModelName = "";
					this.carInfo.otherParam = "";
					this.carInfo.YearName = "";
					this.carInfo.SubYearName = "";
					this.carInfo.stepName = "";
					this.carInfo.lastName = "";
					this.yearlist = [];
					this.subyearlist = [];
					this.carconfiglist = [];
					this.carlastlist = [];
				}
				if (val == 2) { // go=>选年款（大）
					this.step = 2;
					this.carInfo.YearId = 0;
					this.carInfo.YearName = "";
					this.carInfo.SubYearId = 0;
					this.carInfo.SubYearName = "";
					this.carInfo.stepName = "";
					this.carInfo.lastName = "";
					this.subyearlist = [];
					this.carconfiglist = [];
					this.carlastlist = [];
				}
				if (val == 3) { // go=>选年款（小）
					this.step = 3;
					this.carInfo.SubYearId = 0;
					this.carInfo.SubYearName = "";
					this.carInfo.stepName = "";
					this.carInfo.lastName = "";
					this.carconfiglist = [];
					this.carlastlist = [];
				}
				if (val == 4) { // go=>选配置（1）
					this.step = 4;
					this.carInfo.lastName = "";
					this.carlastlist = [];
				}
			},
			close(){
				this.$emit('input',false);
			},
			...mapActions([
			'addCarModelInfo'
			])
		}
	}

</script>

<style>
	#car_main .mint-indexlist-navlist{
		min-height: 400px;
		height: 70%;
	}
	#car_main .mint-indexlist-nav{
		top: -80px;
	}
	#car_main .mint-indexlist-navitem{
		padding: 0 6px;
	}
	#car_main .mint-indexsection-index {
		background: #fff;
	}
	
	#car_main .mint-indexlist-nav {
		border: none;
	}
	
	#car_series {
		margin-bottom: 2rem;
	}
	
	#car_series .mint-header {
		background-color: #fff;
		color: #000;
		border-bottom: solid 1px #dfdfdf;
	}
	
	.car-lb {
		font-size: 0.75rem;
		line-height: 2.6rem;
		padding-left: 0.35rem;
		height: 2.6rem;
		margin-top: 50px;
		overflow: hidden;
	}
	
	.car-logo {
		width: 1.75rem;
		height: 1.75rem;
		vertical-align: middle;
	}
	
	.car-series-name .car-cata {
		width: 100%;
		height: 2rem;
		line-height: 2rem;
		color: #4b9817;
		font-size: 0.6rem;
		background-color: #efefef;
		padding-left: 0.35rem;
	}
	
	.car-series-empty {
		height: 0.825rem;
	}
	
	#car_syear {
		margin-bottom: 2rem;
	}
	
	#car_syear .mint-header {
		background-color: #fff;
		color: #000;
		border-bottom: solid 1px #dfdfdf;
	}
	
	#car_syear .byear-content {
		font-size: 0.75rem;
		margin-top: 2rem;
	}
	
	#car_syear .byear-content .byear-content-title {
		line-height: 2.6rem;
		height: 2.6rem;
		padding-left: 0.35rem;
		border-bottom: solid 5px #dfdfdf;
	}
	
	#car_syear .mui-table-view:before {
		height: 0;
	}
	
	#car_syear .mui-table-view-cell:after {
		left: 0;
	}
	
	#car_fconfig {
		margin-bottom: 2rem;
	}
	
	#car_fconfig .mint-header {
		background-color: #fff;
		color: #000;
		border-bottom: solid 1px #dfdfdf;
	}
	
	#car_fconfig .fconfig-content {
		margin-top: 2rem;
		font-size: 0.65rem;
	}
	
	#car_fconfig .fconfig-content .fconfig-content-title {
		line-height: 1.2rem;
		padding-left: 0.35rem;
		border-bottom: solid 5px #dfdfdf;
	}
	
	#car_fconfig .mui-table-view:before {
		height: 0;
	}
	
	#car_fconfig .mui-table-view-cell:after {
		left: 0;
	}
	
	#car_sconfig {
		margin-bottom: 2rem;
	}
	
	#car_sconfig .mint-header {
		background-color: #fff;
		color: #000;
		border-bottom: solid 1px #dfdfdf;
	}
	
	#car_sconfig .sconfig-content {
		margin-top: 2rem;
		font-size: 0.65rem;
	}
	
	#car_sconfig .sconfig-content .sconfig-content-title {
		line-height: 1.2rem;
		padding-left: 0.35rem;
		border-bottom: solid 5px #dfdfdf;
	}
	
	#car_sconfig .mui-table-view:before {
		height: 0;
	}
	
	#car_sconfig .mui-table-view-cell:after {
		left: 0;
	}
	
	#car_byear {
		margin-bottom: 2rem;
	}
	
	#car_byear .mint-header {
		background-color: #fff;
		color: #000;
		border-bottom: solid 1px #dfdfdf;
	}
	
	#car_byear .byear-content {
		font-size: 0.75rem;
		margin-top: 2rem;
	}
	
	#car_byear .byear-content .byear-content-title {
		line-height: 2.6rem;
		height: 2.6rem;
		padding-left: 0.35rem;
		border-bottom: solid 5px #dfdfdf;
	}
	
	#car_byear .mui-table-view:before {
		height: 0;
	}
	
	#car_byear .mui-table-view-cell:after {
		left: 0;
	}
	
	#car_header .mint-header {
		background-color: #fff;
		color: #000;
		border-bottom: solid 1px #dfdfdf;
	}
	
	#car_header .car-search {
		width: 15rem;
		height: 1.9rem;
		margin: 0.5rem auto;
		border: 1px solid #dfdfdf;
		border-radius: 0.1rem;
		overflow: hidden;
	}
	
	.car-search .car-VIN {
		width: 13rem;
		height: 1.9rem;
		line-height: 1.9rem;
		float: left;
		text-align: left;
		color: #777;
		background: #fff;
	}
	
	.car-search .xcx_search {
		width: 1.3rem;
		height: 1.2rem;
		float: right;
		margin: 0.3rem 0.35rem 0 0;
	}
</style>