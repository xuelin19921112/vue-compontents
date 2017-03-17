<!-- 经销商APP密码重置 -->
<template>
	<div id="password-reset" class="">
		<hearder :title="title"></hearder>
		<div class="content">
			<ul>
				<li class="icon-lock bdb">
					<img src="../../assets/img/login/icon-bf-lock.png" />
					<input type="text" v-model="verifycode"  placeholder="请输入验证码"/>
					<span v-if="timedown">{{timedown}}s</span>
					<span v-else @click="ResetSendCode">重新发送</span>
				</li>
				<li class="icon-password bdb">
					<img src="../../assets/img/login/lock-icon-green.png" />
					<input v-model="newpassword" type="password"  placeholder="请输入您的密码"/>
				</li>
				<li class="icon-password bdb">
					<img src="../../assets/img/login/lock-icon-green.png" />
					<input v-model="comparepassword" type="password"  placeholder="请再次输入您的密码"/>
				</li>
			</ul>
			<div class="btn">
				<span class="mui-btn mui-btn-success reset-btn btn-grey" :class="{'active-btn-green':valid}"  @click="commit" >确定重置</span>
			</div>
		</div>
	</div>
</template>

<script>
import hearder from '../public/header.vue'
import { mapActions, mapGetters } from 'vuex'
import { Toast } from 'mint-ui';
import api from '../../api/urls.js'
import {MD5,Trim,CheckUserName,getcurrDateTime} from '../../content/utils.js';
export default {
	name: 'password-reset',
	components:{
			hearder
	},
	data () {
		return {
			timedown:60,
			phone:"",
			loading:false,
			verifycode:"",
			newpassword:"",
			comparepassword:"",
			title:'密码重置'
		}
	},
	mounted(){
		let _this=this;
        _this.$nextTick(function () {
           _this.countDown();
		})
	},
	beforeRouteEnter (to, from, next) {
		next(vm => {
			vm.phone=vm.$route.params.phone;
		})
	},
	computed:{
        valid(){
           if(Trim(this.verifycode)==""||Trim(this.newpassword)==""||Trim(this.comparepassword)=="")
		   {
			   return false;
		   }
		   return true;
		}
	},
	methods : {
		commit(){
            if(!this.valid || this.loading)
			{
				return false;
			}
			
			if(this.checkpassword()!="")
			{
				  Toast(this.checkpassword());
				  return false;
			}
			let param={
				UserName:this.phone,
				NewPassWord:MD5(this.newpassword),
				VerifyCode:this.verifycode
			}
			let _this=this;
			this.loading=true;
			this.$store.dispatch({
				type : 'postData',
				url : api.ResetPassworld,
				data:param,
				callBack :(data)=>{
					_this.loading=false;
					if(data.status=="success")
					{
						Toast('重置成功');
						_this.$router.push('/');
					}else{
						Toast(data.result);
					}
				}
			})
		},
		checkpassword(){
           if(this.newpassword!=this.comparepassword)
		   {
			   return "两次输入的密码不一致";
		   }
		   return "";
		},
        countDown(){
		   let _this=this;
		   _this.timedown=60;
           var index=setInterval(function(){
				_this.timedown--;
				if(_this.timedown<=0)
				{
					clearInterval(index);
				}
			},1000);
		},
		ResetSendCode(){
			let param={
				Phone:this.phone,
				Type:2
			}
            this.countDown();
			let _this=this;
			this.$store.dispatch({
				type : 'postData',
				url : api.GetVerifiyCode,
				data:param,
				callBack :(res)=>{
					if(res.status=="success")
					{
						  Toast('验证码已发送');
					}
				}
			})
		}
	}
}
</script>
