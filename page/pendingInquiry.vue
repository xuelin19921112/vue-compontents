<!-- 待询价组件 -->
<template>
	<div id="pendingInquiry" class="app">
         <pending-inquiry-hearder @edit="edit" ></pending-inquiry-hearder>
		<!-- 页面主体 -->
		<div class="mian_content">
			<div class="content-main">
				<div v-for="item in list" class="mui-input-row mui-checkbox mui-left list_bd">
					<div class="Y_goods-lists-sub">
						<img :src="item.SmallPic">
						<div>
							<p>
								<a class="ovfh">{{item.ProdBrandName}} {{item.ProdName}}</a>
							</p>
							<p><span>{{item.Sku}}</span></p>
							<p>
								<span class="org">{{item.SalePrice | currency}}</span>
                               <span v-if="item.DealerType==0" class="mui-badge mui-badge-success">品牌件</span>
							   <span v-if="item.DealerType==1&&item.ProdQuality" class="mui-badge mui-badge-success">{{getProdQualityLabel(item.ProdQuality)}}</span>
							</p>
						</div>
					</div>
					<div class="Y_goods-lists-cd">
						<span><img src="../../assets/img/search/dxj_sincerity.png"/>{{formatDealerName(item.DealerName)}}</span>
						<span>产地：<em>{{item.Cus_OriginPlace}}</em></span>
					</div>
					<input name="checkbox" v-model="item.checked" @click="itemcheck" type="checkbox" />
					<div class="mui-input-row input-num">
						<input type="text" v-model="item.MindPrice" placeholder="期望单价">
						<span>购买数量：</span>
						<div class="mui-numbox" data-numbox-min='1'>
							<button class="mui-btn mui-btn-numbox-minus btn-num-minus" type="button" @click="item.Quantity--">-</button>
							<input class="mui-input-numbox" type="number" v-model="item.Quantity" >
							<button class="mui-btn mui-btn-numbox-plus btn-num-plus" type="button" @click="item.Quantity++">+</button>
						</div>
						<div class="mui-input-row">
							<textarea id="textarea" rows="1" placeholder="备注信息" v-model="item.Memo"></textarea>
						</div>
					</div>
				</div>
			    
				<footer class="fiexed-footer" v-if="!IsEdit">
					<div class="input-box">
						<div class="mui-input-row mui-checkbox mui-left">
							<input name="checkbox" v-model="listchecked"  @click="allcheck" type="checkbox" class="qx">
							<label>全选</label>
						</div>
					</div>
					<div class="input-box">
						<div class="mui-input-row mui-checkbox mui-left">
							<input name="checkbox" v-model="IsTax" type="checkbox" class="qx">
							<label>含税</label>
						</div>
					</div>
					<div class="inquiryBtn" style="float: right;">
						<button type="button" class="mui-btn mui-btn-danger btn-grey" :class="{'active-btn-green':valid}" @click="tap()">
						发布询价
					</button>
					</div>
				</footer>
				<footer v-if="IsEdit" class="fiexed-footer">
					<div class="input-box">
						<div class="mui-input-row mui-checkbox mui-left">
							<input name="checkbox" v-model="listchecked" @click="allcheck"  type="checkbox" class="qx">
							<label>全选</label>
						</div>
					</div>
					<div class="inquiryBtn" style="float: right;">
						<button type="button" class="mui-btn mui-btn-danger btn-grey" :class="{'active-btn-green':valid}" @click="deleteAll()">
						删除
					</button>
					</div>
				</footer>
			</div>
			<div class="nothing" v-if="!loading&&!list.length">
				<img src="../../assets/img/inquiry/dxj_no.png" />
				<p>您暂时没有任何待询价信息</p>
				<!--<button type="button" class="mui-btn mui-btn-success">立即前往添加</button>-->
			</div>
		</div>	

	</div>
</template>

<script>
import pendingInquiryHearder from '../pendingInquiry/pendingInquiry_header.vue'
import {getProdQualityLabel,formatDealerName} from 'src/content/utils.js'
import {SourcePlat} from 'src/content/status'
import { mapActions, mapGetters } from 'vuex'
import { Toast } from 'mint-ui';
import api from 'src/api/urls.js'
import { Trim } from 'src/content/utils.js';

export default {
	name: 'pendingInquiry',
	components:{
        pendingInquiryHearder
	},
	data () {
		return {
			list:[],
			loading:false,
			wating:false,
			IsTax:false,
			listchecked:false,
			IsEdit:false
		}
	},
	computed : {
        ...mapGetters(
			{userinfo:'UserInfo'}
		),
		valid(){
			let res=this.list.find(x=>{
				return x.checked==true;
			})
			if(res){
				return true;
			}else{
				return false;
			}
		},
		pricevalid(){
		   let result=true;
			for(let d of this.list)
			{
				if(d.checked &&d.MindPrice!=0 && d.MindPrice<d.SalePrice*0.7)
				{
					result=false;
					break;
				}
			}
		   
		   return result;
		}
	},
	mounted(){
        this.$nextTick(()=>{
              this.query();
		})
	},
	methods : {
		query(){
			let _this=this;
			var param={
				DealerId:this.userinfo.DealerId,
				DealerName:this.userinfo.DealerName,
				PopUserName:this.userinfo.UserName,
				SourcePlat:SourcePlat
			};
			this.loading=true;
			this.$store.dispatch({type : 'postData',url : api.GetInquiryProduct,data:param,callBack :(data)=>{
					_this.loading=false;
					if(data.status=="success")
					{
						let result=JSON.parse(data.result);
						result.forEach(x=>{
							if(x.DealerList.length){
                                x.DealerId=x.DealerList[0].DealerId;
								x.DealerName=x.DealerList[0].DealerName;
								x.DealerType=x.DealerList[0].DealerType;
							}
							x.checked=false;
							if(x.MindPrice==0){
								x.MindPrice="";
							}
						});
						_this.list=result;
						console.log(result);
					}
		    	}
			})
		},
		itemcheck(item){
           item.checked=!item.checked;
			let res=this.list.every(x=>{
				return x.checked==true;
			})
			if(res){
				this.listchecked=true;
			}else{
                this.listchecked=false;
			}
			
		},
        allcheck(){
		   let checked=this.listchecked;
		   this.list.forEach(x=>{
			   x.checked=checked;
		   })
		},
		tap:function(){
			if(!this.valid||this.wating)
			{
				return false;
			}
			if(!this.pricevalid){
				mui.toast('期望价不能低于销售价的70%');
				return false;
			}
			var arr=[];
			this.list.forEach(x=>{
                if(x.checked){
					var obj={Id:x.Id,Quantity:x.Quantity,MindPrice:x.MindPrice,Memo:x.Memo};
					arr.push(obj);
				}
			});
			let param={
				IsTax:this.IsTax?1:0,
				SourcePlat:SourcePlat,
				DealerId:this.userinfo.DealerId,
				DealerName:this.userinfo.DealerName,
				UserName:this.userinfo.UserName,
				PopUserName:this.userinfo.UserName,
				Items:arr
			};
			let _this=this;
			console.log(param);
            this.wating=true;
			this.$store.dispatch({type : 'postData',url : api.PublishInquiryOrder,data:param,callBack :(data)=>{
				    _this.wating=false;
					if(data.status=="success")
					{
						let result=JSON.parse(data.result);
						let newlist=_this.list.filter(x=>{
							return x.checked==false;
						});
						_this.list=newlist;
					    mui.toast('发布成功');
					}else{
						 mui.toast('发布失败,请稍后再试');
					}
		    	}
			})
		},
		deleteAll:function(){
			if(!this.valid)
			{
				return false;
			}
			var arr=this.list.filter(x=>{
				return x.checked==true;
			});
			var btnArray = [];
			arr.forEach(x=>{
				btnArray.push(x.Id);
			})
			let param={
				Ids:btnArray,
				DealerId:this.userinfo.DealerId,
				DealerName:this.userinfo.DealerName,
				PopUserName:this.userinfo.UserName
			}
			let _this=this;
			mui.confirm('确认要删除已选中的配件？', "提醒", function(e) {
				if (e.index == 1) {
					_this.$store.dispatch({type : 'postData',url : api.DeleteInquiryProduct,data:param,callBack :(data)=>{
							if(data.status=="success")
							{
								Toast('删除成功');
							    let newlist=_this.list.filter(x=>{
									return x.checked==false;
								});
								_this.list=newlist;
							}else{
                                Toast('删除失败，请稍后再试');
							}
						}
					})
				}
			})
		},
		getProdQualityLabel(val){
			return getProdQualityLabel(val);
		},
		formatDealerName(val){
			return formatDealerName(val);
		},
		edit(val){
           this.IsEdit=val;
		   this.list.forEach(x=>{
			   x.checked=false;
		   });
		   this.listchecked=false;
		}
	}
}
</script>
<style>
	#pendingInquiry .nothing{
		font-size: 0.7rem;
		line-height: 3rem;
		text-align: center;
		padding-top: 2rem;
	}
	#pendingInquiry .nothing img{
		width: 5.25rem;
		height: 5.25rem;
	}
	#pendingInquiry .nothing .mui-btn-success{
		background: #7bb951;
		border: solid 1px #7bb951;
		padding: 0.6rem 2rem;
	}
	#pendingInquiry .content-main {
		background: #fff;
		padding-bottom: 2.5rem;
	}
	
	.Y_goods-lists-sub {
		min-height: 65px;
		padding: 0.3rem 0.3rem 0.3rem 2.2rem;
		color: #292929;
		overflow: hidden;
	}
	
	.Y_goods-lists-sub img {
		float: left;
		width: 4.68rem;
		height: 4.68rem;
		border: solid 1px #dfdfdf;
		;
	}
	
	.Y_goods-lists-sub>a {
		float: left;
		margin: auto;
		width: 4.68rem;
		height: 4.68rem;
		overflow: hidden;
		border: 1px solid #cccccc;
	}
	
	.Y_goods-lists-sub div {
		padding: 0 10px 0 5.2rem;
	}
	
	.Y_goods-lists-sub div p {
		width: 90%;
		margin-bottom: 0;
		font-size: 0.45rem;
		line-height: 1rem;
	}
	
	.Y_goods-lists-sub div p a {
		color: #151515;
		font-size: 0.63rem;
	}
	
	.Y_goods-lists-sub div p span {
		font-size: 0.45rem;
	}
	
	.Y_goods-lists-sub div p span.org,
	.Y_goods-lists-sub div p span.org em {
		color: #ea7544;
		font-style: normal;
	}
	
	.Y_goods-lists-sub div p span.org em {
		font-size: 0.63rem;
	}
	
	.Y_goods-lists-sub div p span.span-color {
		background: #7bb951;
	}
	
	.Y_goods-lists-sub div p:last-child {
		margin-top: 0.5rem;
	}
	
	.bd_b {
		border-bottom: 1px solid #dfdfdf;
	}
	
	.list_bd {
		border-bottom: 1px solid #dfdfdf;
	}
	
	.list_bd>input {
		top: 30px!important;
	}
	
	.Y_goods-lists-cd {
		font-size: 0.55rem;
		font-style: normal;
		padding: 0.3rem 0.35rem 0.3rem 2.2rem;
		border-bottom: 1px dashed #dfdfdf;
	}
	
	.Y_goods-lists-cd span:first-child {
		display: inline-block;
	}
	
	.Y_goods-lists-cd span:first-child img {
		height: 0.9rem;
		vertical-align: middle;
		margin-right: 4px;
	}
	
	.Y_goods-lists-cd span:last-child {
		float: right;
	}
	
	.Y_goods-lists-cd span em {
		font-style: normal;
	}
	
	.input-num {
		padding: 0.65rem 10px 0.65rem 2.2rem;
	}
	
	.input-num input {
		width: 45%;
		height: 1.75rem;
		border-radius: 0;
		font-size: 0.6rem;
	}
	
	.input-num span {
		font-size: 0.45rem;
		padding-left: 0.2rem;
	}
	
	.input-num .mui-numbox {
		width: 29%;
		margin: 2px 0;
		height: 1.5rem;
	}
	
	#pendingInquiry .btn-num-minus,
	#pendingInquiry .btn-num-plus {
		border-radius: 1.25rem;
		width: 1rem;
		height: 1rem;
		background-color: #777;
		color: #fff;
		top: 0.25rem;
	}
	
	.input-num .mui-numbox {
		border: none;
		padding: 0.9px;
		background: #fff;
	}
	
	.input-num .mui-numbox .mui-input-numbox {
		border-left: none!important;
		border-right: none!important;
	}
	
	.input-num .mui-input-row {
		height: 1.75rem;
		line-height: 0.6rem;
	}
	
	.input-num .mui-input-row textarea {
		height: 1.75rem;
		line-height: 0.85rem;
		overflow: hidden;
		margin-bottom: 0;
		font-size: 0.6rem;
	}
	
	.mui-checkbox input[type=checkbox]:checked:before,
	.mui-radio input[type=radio]:checked:before {
		color: #ea7544;
	}
	
	#pendingInquiry .fiexed-footer {
		width: 100%;
		height: 2.5rem;
		position: fixed;
		left: 0;
		bottom: 0;
		border-top: solid 1px #dfdfdf;
		background: #fff;
	}
	
	#pendingInquiry .mui-checkbox.mui-left label,
	.mui-radio.mui-left label {
		padding-left: 2.2rem;
		padding-right: 0;
	}
	
	#pendingInquiry .input-box {
		float: left;
		width: 4.5rem;
		height: 100%;
		margin-top: 0.3rem;
	}
	
	#pendingInquiry .mui-checkbox.mui-left input[type=checkbox],
	#pendingInquiry .mui-radio.mui-left input[type=radio] {
		left: 0.5rem!important;
	}
	
	#pendingInquiry .mui-btn-danger {
		padding: 0.9rem 1.7rem;
		border-radius: 0;
	}
	/*#pendingInquiry .fiexed-footer .bac-color{
		background: #ea4444;
		border: solid 1px #ea4444;
	}*/
</style>