<!-- 密码修改组件 -->
<template>
	<div id="password-modify" class="">
		<hearder :title="title"></hearder>
		<div class="content">
			<ul>
				<li class="icon-lock bdb">
					<i>*</i>
					<input type="password" v-model="oldpassword"  placeholder="请输入旧密码"/>
				</li>
				<li class="icon-password bdb">
					<i>*</i>
					<input type="password" v-model="newpassword"  placeholder="请输入您的密码"/>
				</li>
				<li class="icon-password bdb">
					<i>*</i>
					<input type="password" v-model="comparepassword"  placeholder="请再次输入您的密码"/>
				</li>
			</ul>
			<div class="btn">
				<span class="mui-btn mui-btn-success reset-btn btn-grey" :class="{'active-btn-green':valid}" @click="commit" >保存</span>
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
	name: 'password-modify',
	components:{
        hearder
	},
	data () {
		return {
			is_fixed : true,
            oldpassword:"",
			newpassword:"",
			comparepassword:"",
			title:'密码修改'
		}
	},
	computed:{
        ...mapGetters(
			{userinfo:'UserInfo'}
		),
        valid(){
           if(Trim(this.oldpassword)==""||Trim(this.newpassword)==""||this.newpassword!=this.comparepassword)
		   {
			   return false;
		   }
		   return true;
		}
	},
	methods : {
		commit(){
           if(!this.valid)
		   {
              return false;
		   }
			let _this=this;
			var param={
				userName:this.userinfo.UserName,
				Password:MD5(this.oldpassword),
				NewPassword:MD5(this.newpassword)
			};
			this.$store.dispatch({type : 'postData',url : api.UpdatePassword,data:param,callBack :(data)=>{
					_this.logining=false;
					if(data.status=="success")
					{
						Toast('修改成功');
						window.localStorage.clear();
						_this.$router.push('/login');
					}else{
						var result=JSON.parse(data.result);
						Toast(result.err_msg);
					}
				}
			})
		},
		checkpassword(){
           if(this.newpassword!=this.comparepassword)
		   {
			   Toast('两次输入的密码不一致');
		   }
		}
		
	}
}
</script>
