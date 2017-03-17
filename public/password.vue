<!-- 经销商APP密码设置 -->
<template>
	<div id="password" class="">
		<mt-header title="密码设置" :fixed="is_fixed" >
			<mt-button  slot="left">
				<a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
			</mt-button>
		</mt-header>
		<div class="content">
			<ul>
				<li class="icon-lock">
					<img src="../../assets/img/login/icon-bf-lock.png" />
					<input v-model="verifycode" type="text"  placeholder="请输入验证码"/>
					<span v-if="timedown">{{timedown}}s</span>
					<span v-else @click="ResetSendCode">重新发送</span>
				</li>
				<li class="icon-password">
					<img src="../../assets/img/login/lock-icon-green.png" />
					<input type="password" v-model="password"  placeholder="请输入您的密码"/>
				</li>
			</ul>
			<div class="btn">
				<span class="mui-btn mui-btn-success register-btn btn-grey"  :class="{'active-btn-green':valid}" @click="commit" >{{loading?"注册中...":"注册"}}</span>
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
	name: 'password',
	data () {
		return {
			timedown:60,
			phone:"",
			password:"",
			loading:false,
			verifycode:"",
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
           if(!this.phone || Trim(this.verifycode)==""||Trim(this.password)=="")
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
			let param={
				UserName:this.phone,
				Company:this.phone,
				PassWord:MD5(this.password),
				Verifycode:this.verifycode
			}
			let _this=this;
			this.loading=true;
			this.$store.dispatch({type : 'postData',url : api.RegisterUser,data:param,callBack :(res)=>{
					 _this.loading=false;
					if(res.status=="success")
					{
						let result=JSON.parse(res.result);
						var nowdate=new Date();
						nowdate.setDate(nowdate.getDate()+1);
                        let user={
							DealerId:result.Id,
							DealerName:result.DealerName,
							DealerType:result.DealerType,
							UserName:_this.phone,
							DealerStatus:result.DealerStatus,
							Address:result.Address,
							IsDirect:result.IsDirect,
							ExpireTime:getcurrDateTime(nowdate)
						}
						localStorage.setItem("userinfo", JSON.stringify(user));
                       _this.addUser(user);
					   _this.$router.push('/registerSuccess');	
					}else
					{
					   Toast(res.result);
					}
				}
			})
		},
		...mapActions([
          'addUser'
		]),
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
				Type:1
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

<style>
	#password{
		margin-bottom: 2rem;
	}
	#password .mint-header{
		background-color: #fff;
		color: #000;
	}
	#password .content ul{
		border-top: solid 1px #dfdfdf;
	}
	#password .content li{
		width: 100%;
		height: 2.2rem;
		line-height: 2.2rem;
		border-bottom: solid 1px #dfdfdf;
		overflow: hidden;
	}
	#password .content .icon-password img{
		width: 1rem;
		height: 1.15rem;
		margin: 0.425rem 0.5rem 0.425rem 0.8rem;
	}
	#password .content .icon-password input,#password .content .icon-lock input{
		float: right;
		width: 13.5rem;
		height: 2.2rem;
		line-height: 2.2rem;
		border: none;
		outline: none;
	}
	#password .content .icon-lock{
		position: relative;
	}
	#password .content .icon-lock img{
		width: 1.2rem;
		height: 0.9rem;
		margin: 0.65rem 0.5rem 0.65rem 0.7rem;
	}
	#password .content .icon-lock span{
		position: absolute;
		top: 0.3rem;
		right: 1rem;
		font-size: 0.65rem;
		line-height: 1.4rem;
		padding: 0 1rem;
		border: solid 1px #7bb951;
	}
	#password .content .register-btn{
		width: 15rem;
		height: 2.1rem;
		line-height: 1.5rem;
		/*background: #7bb951;
		border: solid 1px #7bb951;*/
		/*background: #d1d1d1;
		border: solid 1px #d1d1d1;*/
		margin: 2.5rem 0.5rem;
	}
</style>