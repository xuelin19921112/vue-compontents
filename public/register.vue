<!-- 北迈账号注册组件 -->
<template>
	<div id="register" class="">
		<div v-show="!showAgreement">
			<hearder :title="title"></hearder>
			<div class="register-content">
				<div class="input-group-register">
					<ul>
						<li class="icon-phone bdb">
							<img src="../../assets/img/login/dealer_logo.png"  style="width: 1.1rem;height: 1.2rem;margin: 0.6rem 0.5rem 0.6rem 0.8rem;"/>
							<input v-model="phone" type="number" placeholder="请输入企业名称" />
						</li>
						<li class="icon-phone bdb">
							<img src="../../assets/img/login/iconfont-bf-mobile.png" />
							<input v-model="phone" type="number" placeholder="请输入手机号" />
						</li>
					</ul>
					<div class="btn">
						<!--<router-link to="/password">-->
						<span class="mui-btn mui-btn-success lock-btn btn-grey" :class="{'active-btn-green':checkMobile&&isagree}" @click="SendCode">{{loading?'获取中...':'获取验证码'}}</span>
						<!--</router-link>-->
					</div>
					<div class="mui-input-row mui-checkbox mui-left" style="position: relative;">
						<label>已阅读并同意</label>
						<a class="agreement" @click="showAgreement=true"><span class="font-color-green">用户服务协议</span></a>
						<input name="checkbox" v-model="isagree" type="checkbox">
					</div>
				</div>
			</div>
		</div>
		<user-agreement  v-model="showAgreement"></user-agreement>
	</div>
</template>
<script>
	import hearder from '../public/header.vue'
    import userAgreement from './user_agreement.vue'
	import { mapActions, mapGetters } from 'vuex'
	import { Toast } from 'mint-ui';
	import api from '../../api/urls.js'
	import { MD5, Trim, CheckUserName, getcurrDateTime } from '../../content/utils.js';

	export default {
		name: 'register',
		data() {
			return {
				is_fixed: true,
				loading: false,
				isagree:true,
				phone: "",
				showAgreement:false,
				title:'北迈账号注册'
			}
		},
		components:{
			hearder,userAgreement
		},
		computed: {
			checkMobile() {
				var reg = /^(\+86)?1[3,4,5,6,7,8,9](\d{9})$/;
				return reg.test(this.phone);
			}
		},
		methods: {
			SendCode() {
                // this.$router.push({ name: 'password', params: { phone: this.phone } });
				if (!this.checkMobile || !this.isagree || this.loading) {
					return false;
				}
				let param = {
					Phone: this.phone,
					Type: 1
				}
				let _this = this;
				this.loading = true;
				this.$store.dispatch({ type: 'SendVerifyCode', url: api.GetVerifiyCode, data: param }).then(res => {
					_this.loading = false;
					if (res.status == "success") {
						_this.$router.push({ name: 'password', params: { phone: _this.phone } });
					}else
					{
                       Toast(res.result);
					}
					
				});
			}
		}
	}

</script>
