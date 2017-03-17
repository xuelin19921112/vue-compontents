<!-- 经销商APP密码找回 -->
<template>
	<div id="password-recovery" class="">
		<hearder :title="title"></hearder>
		<div class="content">
			<ul>
				<li class="icon-phone bdb">
					<img src="../../assets/img/login/iconfont-bf-mobile.png" />
					<input type="number" v-model="phone"  placeholder="请输入手机号"/>
				</li>			</ul>
			<div class="btn">
				<!--<router-link to="/passwordReset">-->
			    	<span class="mui-btn mui-btn-success lock-btn btn-grey" :class="{'active-btn-green':checkMobile}" @click="GetCode" >{{loading?'获取中...':'获取验证码'}}</span>
				<!--</router-link>-->
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
	name: 'password-recovery',
	components:{
			hearder
	},
	data () {
		return {
			phone:"",
			verifiyCode:"",
			loading:false,
			title:'密码找回'
		}
	},
	computed:{
	   checkMobile(){
          var reg=/^(\+86)?1[3,4,5,6,7,8,9](\d{9})$/;
          return reg.test(this.phone);
	   },
       valid(){
		   if(Trim(this.phone)=="")
		   {
              return false;
		   }
		   return true;
	   }
	},
	methods : {
		GetCode(){
			//  this.$router.push({name:'passwordReset',params:{phone:this.phone}});
			if(!this.checkMobile||this.loading)
			{
				return false;
			}
			let param={
				Phone:this.phone,
				Type:2
			}
			let _this=this;
			this.loading=true;
			this.$store.dispatch({
				type : 'postData',
				url : api.GetVerifiyCode,
				data:param,
				callBack :(data)=>{
					_this.loading=false;
					if(data.status=="success")
					{
					   _this.$router.push({name:'passwordReset',params:{phone:_this.phone}});
					}
				   console.log(data);
				}
			})
		}
	}
}
</script>

