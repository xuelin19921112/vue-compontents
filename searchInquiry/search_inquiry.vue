<!--找配件询价单组件-->
<template>
	<div v-show="value" class="">
		<div v-show="!showtip">
			<mt-header title="询价单" :fixed="is_fixed" class="inquiry-title">
				<mt-button slot="left">
					<a class=" mui-icon mui-icon-left-nav mui-pull-left" @click="close"></a>
				</mt-button>
			</mt-header>
			<div class="inquiry-part-info bottom-line">
				<p class="inquiry-part-title font-color-red">配件信息</p>
				<p>{{prodInfo.ProdBrandName}} {{prodInfo.ProdName}} {{prodInfo.Sku ? ('('+prodInfo.Sku+')'):''}}</p>
				<p>参考价:<span class="font-small font-color-red">{{prodInfo.DealerType==0?prodInfo.TradePrice:prodInfo.InPrice | currency}}</span></p>
				<p>品质：{{GetQuality(prodInfo.ProdQuality)}} 产地：{{prodInfo.Cus_OriginPlace}}</p>
				<div class="inquiry-sin">
					<img class="inquiry-sincerity" src="../../assets/img/search/dxj_sincerity.png">
					<p class="inquiry-service">{{formatDealerName(prodInfo.DealerName)}}</p>
				</div>
			</div>
			<div class="inquiry-need">
				<p class="inquiry-part-title font-color-red">需求数</p>
				<numbox v-model="prodInfo"></numbox>
				<!--<div class="mui-numbox" data-numbox-min='1' style="margin: 0 0 0.5rem 0;">
					<button class="mui-btn mui-btn-numbox-minus" type="button" @click="prodInfo.quantity--">-</button>
					<input id="test" class="mui-input-numbox" type="number" v-model.number="prodInfo.quantity" />
					<button class="mui-btn mui-btn-numbox-plus" type="button" @click="prodInfo.quantity++">+</button>
				</div>-->
			</div>
			<div class="inquiry-price bottom-line">
				<p class="inquiry-part-title font-color-red">期望价格<span>(单价)</span></p>
				<input type="text" placeholder="请输入你的心理价位供商家具体报价" v-model="MindPrice" class="inquiry-account">
				<p class="inquiry-notice bottom-line">价格不能低于当前零售价的70%。也可不填，由该商家具体报价</p>
				<div class="mui-input-row mui-checkbox mui-left">
					<input v-model="IsTax" name="checkbox" value="" type="checkbox" class="qx">
					<label>是否含税</label>
				</div>
			</div>
			<div class="inquiry-att">
				<p class="inquiry-part-title font-color-red">备注</p>
				<textarea class="inquiry-box" v-model="PurchaseMemo" placeholder="备注信息最多不超过100个字"></textarea>
			</div>
			<div class="inquiry-footer">
				<button class="inquiry-add btn-grey"  @click="commit(1)">加入询价单</button>
				<button class="inquiry-now pull-right btn-grey"  @click="commit(2)">立即询价</button>
			</div>
		</div>
		<inquerytip v-model="showtip" :title="type==1?'发送成功':'保存成功'">
			<template v-if="type==1">
				<span>配件已经成功加入询价单</span>
				<router-link to="/pendingInquiry" slot="s1">前往待询价清单查看</router-link>
				 <a @click="backsearch" slot="s2">继续询价</a>
			</template>
			<template v-if="type==2">
				<p>您的询价信息已经发送成功</p>
		     	<p>请耐心等待经销商的回复</p>
			   <router-link to="/pubInquiry" slot="s1">前往询价单查看</router-link>
			   <a @click="backsearch" slot="s2">继续询价</a>
			</template>
		</inquerytip>
	</div>
</template>

<script>
    import numbox from 'components/common/numbox.vue'
    import eventHub from 'src/bus'
    import { Toast } from 'mint-ui';
	import { mapActions, mapGetters } from 'vuex'
	import api from 'src/api/urls.js'
	import { ProdQuality } from 'src/content/status.js'
	import { formatDealerName } from 'src/content/utils.js'
	import inquerytip from 'components/common/inquerytip'
	export default {
		name: 'search_inquiry',
		components: {
			inquerytip,numbox
		},
		data() {
			return {
				showtip:false,
				is_fixed: true,
				MindPrice: "",
				IsTax: false,
				type:1,
				PurchaseMemo: ""
			}
		},
		props: {
			prodInfo: {
				type: Object,
				default: function () {
					return {};
				}
			},
			value: {
				type: Boolean,
				default: false
			}
		},
		computed: {
			...mapGetters(
				{userinfo:'UserInfo'}
			)
		},
		methods: {
			close() {
				this.$emit('input', false);
			},
			backsearch(){
			   this.showtip=false;
			   this.$emit('input', false);
               eventHub.$emit('goodsinfo',false);
			},
			GetQuality(val) {
				var item = ProdQuality.find(x => {
					return x.value == val
				});
				return item ? item.label : '--';
			},
			commit(type) {
				if(this.prodInfo.quantity<=0){
					Toast('数量不能小于1');
					return false;
				}
				let _this = this;
				var detail = JSON.parse(JSON.stringify(this.prodInfo));
				detail.Memo = this.PurchaseMemo;
				detail.DealerIds = [detail.DealerId];
				detail.MindPrice = this.MindPrice;
				detail.SalePrice = detail.DealerType == 0 ? detail.TradePrice : detail.InPrice;
				var param = {
					AddType: type,
					DealerId: this.userinfo.DealerId,
					DealerName: this.userinfo.DealerName,
					IsTax: this.IsTax ? 1 : 0,
					UserName: this.userinfo.UserName,
					Items: [detail]
				};
				this.$store.dispatch({type : 'postData',url : api.AddInquiryProduct,data:param,callBack :(data)=>{
						if(data.status=="success")
						{
							_this.type=type;
							_this.showtip=true;
						}else
						{
							Toast(data.result);
						}
						console.log(data);
					}
				})

			},
			formatDealerName(val) {
				if (!val) return "";
				return formatDealerName(val);
			}
		}
	};

</script>
<style>
	.bottom-line {
		border-bottom: 1px solid #dfdfdf;
	}
	
	.pull-left {
		float: left;
	}
	
	.pull-right {
		float: right;
	}
	
	.inquiry-title {
		background-color: #ffffff;
		border-bottom: 1px solid #dfdfdf;
		color: #000;
	}
	
	.inquiry-part-info {
		margin-top: 2rem;
		padding-left: 0.35rem;
	}
	
	p {
		color: #343434;
	}
	
	.inquiry-part-title {
		font-size: 0.75rem;
		line-height: 2rem;
	}
	
	.inquiry-sin {
		overflow: hidden;
		padding: 0.5rem 0;
	}
	
	.inquiry-service {
		padding-left: 0.2rem;
	}
	
	.inquiry-sincerity {
		width: 1rem;
		height: 0.9rem;
		float: left;
	}
	
	.inquiry-need {
		padding-left: 0.35rem;
		border-bottom: 1rem;
		margin-bottom: 1rem;
		border-bottom: 1px solid #ccc;
		overflow: hidden;
	}
	
	.inquiry-number input {
		width: 1.97rem;
		height: 1.4rem;
		border-left: none;
		border-right: none;
		border-radius: 0;
		appearance: none;
		padding: 2px 0;
	}
	
	.inquiry-num {
		color: #d7d7d7;
		text-align: center;
		border: 1px solid #d7d7d7;
		-webkit-border-radius: none;
		-webkit-appearance: none;
	}
	
	.inquiry-price {
		padding: 0 0.35rem 0.25rem 0.35rem;
		margin-top: -1.2rem;
		overflow: hidden;
	}
	
	.inquiry-price span {
		color: #151515;
		font-size: 0.7rem;
	}
	
	.inquiry-account {
		height: 2.2rem!important;
		line-height: 2.2rem!important;
		display: block;
		font-size: 0.6rem;
		text-align: left;
		margin-right: 0.275rem;
		background: #efefef!important;
	}
	
	.inquiry-price .inquiry-notice {
		color: #c7c7c7;
		font-size: 0.5rem;
		line-height: 1rem;
		margin-top: -0.49rem;
		padding-bottom: 0.25rem;
	}
	
	.inquiry-price .inquiry-check {
		width: 1.125rem;
		height: 0.85rem;
	}
	
	.inquiry-att {
		padding: 0 0.35rem 2rem 0.35rem;
	}
	
	.inquiry-box {
		color: #c7c7c7;
		font-size: 0.4rem;
	}
	
	.inquiry-footer {
		width: 100%;
		border: none;
		-webkit-border: none;
		position: fixed;
		bottom: 0;
		left: 0;
	}
	
	.inquiry-footer .inquiry-add {
		width: 50%;
		height: 2.5rem;
		color: #fff;
		font-size: 0.75rem;
		border-radius: 0;
		background-color: #7bb951;
		border: solid 1px #7bb951;
	}
	
	.inquiry-footer .inquiry-now {
		width: 50%;
		height: 2.5rem;
		color: #fff;
		font-size: 0.75rem;
		border-radius: 0;
		background-color: #ea7544;
		border: solid 1px #ea7544;
	}
	
	#search_inquiry .mui-checkbox.mui-left input[type=checkbox],
	.mui-radio.mui-left input[type=radio] {
		left: 0;
	}
	
	#search_inquiry .mui-checkbox.mui-left label,
	.mui-radio.mui-left label {
		padding-left: 35px;
	}
</style>