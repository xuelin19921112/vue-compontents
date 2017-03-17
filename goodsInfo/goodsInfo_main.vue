<!-- 配件详情主体组件 -->
<template>
	<div id="goodsInfo_main" v-show="value">
		<div v-show="!detailshow&&!inqueryshow">
			<div>
				<mt-header title="配件详情" :fixed="is_fixed">
					<mt-button  slot="left">
						<a class="mui-icon mui-icon-left-nav mui-pull-left" @click="close"></a>
					</mt-button>
				</mt-header>
			</div>
			<div class="mian_content">
			   <div style="height: 13.5rem;" v-show="ImgList.length>0">
					<mt-swipe :auto="4000">
						<mt-swipe-item v-for="img in ImgList" >
							<a href="#">
								<img :src="img">
							</a>
						</mt-swipe-item>
					</mt-swipe>
			   </div>
				<div style="height: 13.5rem;" v-show="ImgList.length==0">
					<img src="../../assets/img/search/search_nopic.png" style="width: 100%;height: 13.5rem;" class="bdb"/>
			   	</div>
				<div class="goods-info">
					<p style="line-height: 1.5rem;font-size: 0.75rem;">
						<span class="font-color">{{prodInfo.ProdBrandName}}</span> {{prodInfo.ProdName}}{{prodInfo.Sku ? ('('+prodInfo.Sku+')'):''
						}}
					</p>
					<p style="font-size: 0.65rem;line-height: 1rem;">
						参考价：
						<span class="font-color">{{prodInfo.DealerType==0?prodInfo.TradePrice:prodInfo.InPrice | currency}}</span>
					</p>
					<p style="font-size: 0.65rem;line-height: 1rem;">
						<span>品质：{{GetQuality(prodInfo.ProdQuality)}}</span>
						<span style="padding-left: 1rem;">产地：{{prodInfo.Cus_OriginPlace}}</span>
					</p>
				</div>
				<div class="business-info">
					<p class="business-name">
						<img src="../../assets/img/search/dxj_sincerity.png" />
						<span>{{formatDealerName(prodInfo.DealerName)}}</span>
					</p>
					<!--<p>
					售后说明：
					<span>售后说明售后说明售后说明售后说明售后说明售后说明售后说明</span>
				</p>-->
				</div>
				<div class="use-info">
					<p class="use-title">适用性</p>
					<div class="use-info-title">
						<p class="font-color">配件说明</p>
						<p>{{prodInfo.ContentInfo}}</p>
					</div>
					<div class="use-info-title">
						<p class="font-color">替换号</p>
						<p>{{replacesku}}</p>
					</div>
					<div class="use-info-title">
						<p class="font-color">适用车型信息</p>
						<div class="mui-card">
							<ul class="mui-table-view">
								<li v-for="item in SuitCarList" class="mui-table-view-cell mui-collapse">
									<a class="mui-navigate-right border-bottom" href="#">{{item.FactoryName}}</a>
									<ul class="mui-collapse-content">
										<li v-for="car in item.carlist"><a @click="ToDetail(car)">{{item.FactoryName}}-{{car.Carmodel}}<img src="../../assets/img/search/xcx_nextstep.png"/></a></li>
									</ul>
								</li>
							</ul>
						</div>
					</div>
				</div>
				<!--<div class="has-nothing">
				没有更多内容了
			</div>-->
			</div>
			<div class="footer bdt">
				<numbox v-model="prodInfo"></numbox>
				<!--<div class="mui-numbox" data-numbox-min='0' >
					<button class="mui-btn mui-btn-numbox-minus" type="button" @click="prodInfo.quantity--">-</button>
					<input id="test" class="mui-input-numbox" v-model="prodInfo.quantity" type="number" />
					<button class="mui-btn mui-btn-numbox-plus" type="button" @click="prodInfo.quantity++">+</button>
				</div>-->
				<!--<router-link to="/searchInquiry">-->
					<button type="button" class="inquiryBtn btn-grey" :class="{'active-btn-green':prodInfo.quantity>0}" @click="ToAsk" >
					立即前往询价
				</button>
				<!--</router-link>-->
			</div>
		</div>

		<use-to v-model="detailshow" :title="title" :list="yearlist"></use-to>
		<search-inquiry v-model="inqueryshow" :prodInfo="prodInfo" ></search-inquiry>
	</div>
</template>

<script>
    import numbox from 'components/common/numbox.vue'
	import useTo from '../useTo/useTo.vue'
	import searchInquiry from 'components/searchInquiry/search_inquiry'
	import { mapActions, mapGetters } from 'vuex'
	import { ProdQuality } from '../../content/status.js'
	import { formatDealerName } from '../../content/utils.js'
	import api from '../../api/urls.js'
	export default {
		name: 'goodsInfo_main',
		components: {
			useTo,searchInquiry,numbox
		},
		props: {
			value:{
				type:Boolean,
				default:false
			},
			prodInfo: {
				type: Object,
				default: function () {
					return {}
				}
			}
		},
		data() {
			return {
				replacesku: "",
				SuitCarList: [],
				SkuList: [],
				StockId: 0,
				Paras: {},
				title: "",
				yearlist: [],
				detailshow: false,
				inqueryshow:false,
				ImgList: []
			}
		},
		computed: {
        ...mapGetters(
				{userinfo:'UserInfo'}
			)
		},
		mounted() {
			this.$nextTick(() => {
				this.slider();
			})
		},
		watch: {
			value(val){
				if(val)
				{
					window.scrollTo(0,0);
				}
			},
			prodInfo() {
				console.log('w');
				this.Getparas();
				this.GetImgList();
			}
		},
		methods: {
			Getparas() {
				let _this = this;
				var param = {
					StockId: this.prodInfo.StockId
				};
				this.$store.dispatch({
					type: 'postData', url: api.popGetparas, data: param, callBack: (data) => {
						if (data.status == "success") {
							let result = JSON.parse(data.result);
							_this.Paras = result;
							if (result.SkuList.length) {
								_this.replacesku = result.SkuList.join(',');
							}
							if (result.SuitCarList.length) {
								_this.SuitCarList = _this.FormatSuitCar(result.SuitCarList);
							}
						}
					}
				})
			},
			ToDetail(item) {
				var arr = [];
				this.title = item.FactoryName + '-' + item.Carmodel,
					item.yearlist.forEach(x => {
						var obj = { text: x.PartsYear, desc: x.Description }
						arr.push(obj);
					});
				this.yearlist = arr;
				this.detailshow = true;
			},
			ToAsk(){
               if(this.quantity<=0)
			   {
				   return false;
			   }
			   this.inqueryshow=true;

			},
			reduce(){
               if(this.prodInfo.quantity>0)
			   {
				   this.prodInfo.quantity--;
			   }
			},
			GetImgList() {
				let _this = this;
				var param = {
					DealerId: this.userinfo.DealerId,
					StockId: this.prodInfo.StockId
				};
				this.$store.dispatch({
					type: 'postData', url: api.GetProductImageList, data: param, callBack: (data) => {
						if (data.status == "success") {
							let result = JSON.parse(data.result);
							_this.ImgList = result;
						}
					}
				})
			},
			FormatSuitCar(suitcars) {
				var reslist = [];
				let facset = new Set();
				let carIdset = new Set();
				suitcars.map(x => {
					facset.add(x.FactoryId);
					carIdset.add(x.CarmodelId);
				});
				var carmodellist = [];
				carIdset.forEach(x => {
					var yearlist = suitcars.filter(f => {
						return f.CarmodelId == x;
					})
					var obj = { FactoryId: yearlist[0].FactoryId, FactoryName: yearlist[0].FactoryName, CarmodelId: x, Carmodel: yearlist[0].Carmodel, yearlist: yearlist };
					carmodellist.push(obj);
				});

				for (let item of facset) {

					var faccars = carmodellist.filter(f => {
						return f.FactoryId == item;
					})
					var facname = faccars[0].FactoryName;
					var obj = { FactoryId: item, FactoryName: facname, carlist: faccars }
					reslist.push(obj);
				}
				return reslist;

			},
			GetQuality(val) {
				var item = ProdQuality.find(x => {
					return x.value == val
				});
				return item ? item.label : '--'

			},
			formatDealerName(val) {
				if (!val) return "";
				return formatDealerName(val);
			},
			slider() {
				mui.init();
				var slider = mui("#slider");
				slider.slider({
					interval: 2000
				});
			},
			close(){
				this.$emit('input',false);
			},
			getNumbox(msg){
				console.log(msg);
				this.prodInfo.quantity=msg;
			}
		}
	}

</script>