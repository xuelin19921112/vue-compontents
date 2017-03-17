<!-- 经销商APP登录页面 -->
<template>
	<div id="login" class="app">
		<div class="login-content">
			<div class="login-phone">
				<span class="icon-phone"><img src="../../assets/img/login/icon-mobile.png" /></span>
				<input type="number" v-model.number="username" placeholder="请输入手机号" />
			</div>
			<div class="login-lock">
				<span class="icon-lock"><img src="../../assets/img/login/lock-icon.png" /></span>
				<input type="password" v-model="password" placeholder="请输入密码" />
			</div>
			<div class="btn">
				<span class="mui-btn mui-btn-success login-btn btn-grey" :class="{'active-btn-green':valid}" @click="commit" >{{logining?'登录中...':'登录'}}</span>
			</div>
			<div class="footer">
				<router-link to="/register"><span class="register">手机快速注册</span></router-link>
				<router-link to="/passwordRecovery"><span class="recovery">忘记密码</span></router-link>
			</div>
		</div>
	</div>
</template>

<script>
import { mapActions, mapGetters } from 'vuex'
import { Toast } from 'mint-ui';
import api from '../../api/urls.js'
import {MD5,Trim,CheckUserName,getcurrDateTime} from '../../content/utils.js';
export default {
	name: 'login',
	data () {
		return {
			username:"",
			password:"",
			logining:false
		}
	},
	computed:{
       valid(){
		   if(Trim(this.username)==""||Trim(this.password)=="")
		   {
              return false;
		   }
		   return true;
	   }
	},
	methods : {
		commit(){
			if(!this.valid||this.logining)
			{
				return false;
			}
			var username=Trim(this.username);
			if(!CheckUserName(username))
			{
               Toast('用户名格式错误');
			   return false;
			}
			let param={
				userName:this.username,
				password:MD5(this.password)
			}
			let _this=this;
			this.logining=true;
			try {
				this.$store.dispatch({type : 'postData',url : api.LoginSys,data:param,callBack :(data)=>{
						_this.logining=false;
						if(data.status=="success")
						{
							let result=JSON.parse(data.result);
							var nowdate=new Date();
							nowdate.setDate(nowdate.getDate()+1);
							let user={
								DealerId:result.Id,
								DealerName:result.DealerName,
								DealerType:result.DealerType,
								UserName:_this.username,
								DealerStatus:result.DealerStatus,
								Address:result.Address,
								IsDirect:result.IsDirect,
								ExpireTime:getcurrDateTime(nowdate)
							}
							localStorage.setItem("userinfo", JSON.stringify(user));
						_this.addUser(user);
						_this.$router.push('/');
					}else{
							var result=JSON.parse(data.result);
							Toast(result.err_msg);
						}
					}
				})
			}catch (e)
			{
				alert(e);
        	}
		},
		...mapActions([
          'addUser'
		]),
		toast:function(){
			mui.toast('提示错误原因');
		}
	}
}
</script>
