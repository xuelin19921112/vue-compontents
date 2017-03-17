<!-- 企业信息组件 -->
<template>
	<div id="dealer-info" class="">
		<hearder :title="title"></hearder>
		<div class="content">
			<div class="Authentication_user">
				<div class="touxiang text-center">
					<img src="../../assets/img/my/touxiang.png" class="touxiang-img"/>
					<mt-button @click.native="sheetVisible = true" size="small" class="active-btn-red">更换头像</mt-button>
				</div>
				<div class="mui-content-padded" style="">
					<p><span>认证成为经销商后，您将获得更多的操作权限</span></p>
				</div>
				<div>
					<form class="mui-input-group">
						<div class="mui-input-row">
							<label>账号</label>
							<input type="text" ref="dealer-accout">
						</div>
						<div class="mui-input-row">
							<label class="color_777"><i> * </i>企业名称</label>
							<input type="text" class="mui-input-clear overflow"  data-input-clear="5" ref="dealer-name">
						</div>
						<div class="mui-input-row color_777">
							<label><i> * </i>联系人</label>
							<input type="text" ref="dealer-contact">
						</div>
						<div class="mui-input-row color_777">
							<label>QQ</label>
							<input type="text" ref="dealer-qq">
						</div>
						<div class="mui-input-row mui-plus-hidden color_777">
							<label style="width: 50%;"><i> * </i><span id="cityResult3">企业所在区域</span></label>
							<span id='showCityPicker3' @click="selectArea()"></span>
						</div>
						<div class="mui-input-row color_777">
							<label>详细地址</label>
							<input type="text" ref='address-detail'>
						</div>
						<div class="checkbox_list">
							<label style="line-height: 1.8rem;"><i> * </i>请选择企业类型</label>
							<ul class="mui-input-row">
								<li>
									<div class="mui-input-row mui-checkbox mui-left">
										<input name="checkbox" value="" type="checkbox" checked=""><span>经销商</span>
									</div>
								</li>
								<li>
									<div class="mui-input-row mui-checkbox mui-left">
										<input name="checkbox" value="" type="checkbox" checked=""><span>修理厂</span>
									</div>
								</li>
								<li>
									<div class="mui-input-row mui-checkbox mui-left">
										<input name="checkbox" value="" type="checkbox" checked=""><span>其它</span>
									</div>
								</li>
							</ul>

						</div>
						<div class="mui-input-row color_777">
							<label>售后说明</label>
							<input type="text" class="overflow" ref=''>
						</div>
						<div class="feedback">
							<div id="image-list" class="mui-input-row image-list file-img">
								<label><i> * </i>营业执照</label>
								<!--<div class="image-item space">
									<div class="image-close">X</div>
									<div class="image-up"></div>
									<div class="file" id="image-1"></div>
								</div>-->
								<mt-button @click.native="sheetVisible = true" size="small"></mt-button>
								<mt-button @click.native="sheetVisible = true" size="small"></mt-button>
							</div>
						</div>
						<div class="mui-input-row color_777">
							<label><i> * </i>开户行</label>
							<input type="text" class="overflow">
						</div>
						<div class="mui-input-row color_777">
							<label><i> * </i>开户行名称</label>
							<input type="text" class="overflow">
						</div>
						<div class="mui-input-row color_777">
							<label><i> * </i>账号</label>
							<input type="text" class="overflow">
						</div>
						<div class="mui-input-row color_777">
							<label>备注</label>
							<input type="text" class="overflow">
						</div>
						<div class="mui-content-padded btn_bc">
							<button type="button" class="mui-btn mui-btn-warning mui-btn-block" @click="DealerCommitAudit()">保存</button>
						</div>
					</form>
				</div>

			</div>
			<mt-actionsheet :actions="actions" v-model="sheetVisible"></mt-actionsheet>
		</div>
	</div>
</template>
<script>
	import { Actionsheet } from 'mint-ui';
	import { Picker } from 'mint-ui';
	import { mapGetters } from 'vuex'
	import api from '../../api/urls.js'
	import {MD5,Trim,CheckUserName,getcurrDateTime} from '../../content/utils.js';
	import hearder from '../public/header.vue'
	export default {
		name: 'dealer-info',
		components:{
        	hearder
		},
		data() {
			return {
				title:'企业信息',
				is_fixed: true,
				sheetVisible: false,
				actions: []
				}
		},
		computed : {
	        ...mapGetters(
				{userinfo:'UserInfo'}
			)
		},
		mounted(){
//	        this.$nextTick(()=>{
//	              this.DealerCommitAudit();
//			})
			this.actions = [{
			    name: '拍照',
			    method: this.takePhoto
			    }, {
			    name: '从相册中选择',
			    method: this.openAlbum
			}];
		},
		methods: {
			takePhoto() {
		        console.log('taking photo');
		     },
		    openAlbum() {
		        console.log('opening album');
		    },
		    selectArea:function(){
		    	(function($, doc) {
					$.init();
					$.ready(function() {
						var cityPicker3 = new $.PopPicker({
							layer: 3
						});
						cityPicker3.setData(cityData3);
						var showCityPickerButton = doc.getElementById('showCityPicker3');
						var cityResult3 = doc.getElementById('cityResult3');
							cityPicker3.show(function(items) {
								cityResult3.innerText = "你选择的城市是:" + (items[0] || {}).text + " " + (items[1] || {}).text + " " + (items[2] || {}).text;
								//返回 false 可以阻止选择框的关闭
								//return false;
							});
					});
			})(mui, document);
		    },
			DealerCommitAudit:function(){
				let _this=this;
				var param={
					UserName:this.userinfo.UserName,
					Avatar:'touxiang',
					DealerId:this.userinfo.DealerId
					
				};
				console.log(param.DealerName);
				this.$store.dispatch({type : 'postData',url : api.DealerCommitAudit,data:param,callBack :(data)=>{
						_this.logining=false;
						if(data.status=="success")
						{
							let result=JSON.parse(data.result);
							console.log(result);
//							this.maindata=[result.productCount,result.inquiryOrderCount,result.orderCount]
						}
					}
				})
			}
			
		}
	}
</script>
