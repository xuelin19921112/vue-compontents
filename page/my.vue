<!-- 个人中心主页组件 -->
<template>
	<div id="my" class="app">
		<!-- 顶部 -->
        <hearder :title="title"></hearder>
        
		<!-- 页面主体 -->
        <div id="my_main" class="">
			<div class="mian_content">
				<div class="title text-center">
					<img src="../../assets/img/my/touxiang.png"  class="touxiang"/>
					<p>
						<span class="account">{{userinfo.UserName}}</span>
						<img v-if="userinfo.DealerStatus==0" src="../../assets/img/my/1.3.png" class="renzheng"/> 
						<img v-if="userinfo.DealerStatus==1" src="../../assets/img/my/1.4.png" class="renzheng"/>
						<img v-if="userinfo.DealerStatus==2" src="../../assets/img/my/1.1.png" class="renzheng"/>
						<img v-if="userinfo.DealerStatus==3" src="../../assets/img/my/1.2.png" class="renzheng"/> 
					</p>
					<p>
						<router-link to="/dealerInfo">
							<span class="account-name">{{userinfo.DealerName}}</span>
							<img src="../../assets/img/search/xcx_nextstep.png" class="icon-next"/>
						</router-link>
					</p>
				</div>
				<ul class="mui-table-view">
					<li class="mui-table-view-cell">
						<router-link to="/news">
							<a class="mui-navigate-right">
								<img src="../../assets/img/my/info.png" class="icon-info" />
								 消息
							</a>
						</router-link>
					</li>
					<li class="mui-table-view-cell">
						<router-link to="/passwordModify">
							<a class="mui-navigate-right">
								<img src="../../assets/img/my/lock.png" class="icon-lock"/>
								 密码修改
							</a>
						</router-link>
					</li>
					<li class="mui-table-view-cell">
						<router-link to="/about">
							<a class="mui-navigate-right">
								<img src="../../assets/img/my/about.png" class="icon-about"/>
								 关于
							</a>
						</router-link>
					</li>
				</ul>
				<button type="button" class="mui-btn btn-grey sign-out" @click="SignOut">退出登录</button>
			</div>	
	</div>
        
		<!-- 底部导航栏 -->
        <myfooter></myfooter>
	</div>
</template>

<script>
import hearder from '../public/header.vue'
import myfooter from '../my/my_footer.vue'
import { mapGetters } from 'vuex'
import api from '../../api/urls.js'
import {MD5,Trim,CheckUserName,getcurrDateTime} from '../../content/utils.js';
import { Toast } from 'mint-ui';
export default {
	name: 'my',
	components:{
        hearder,myfooter
	},
	data () {
		return {
			title:'我的'
		}
	},
	computed : {
        ...mapGetters(
			{userinfo:'UserInfo'}
		)
	},
	methods : {
		SignOut(){
			window.localStorage.clear();
			Toast('注销成功');
			this.$router.push('/login');
		}
	}
}
</script>
