<!-- 关于组件 -->
<template>
	<div id="about" class="">
		<hearder :title="title"></hearder>
		<div class="about-content">
			<ul class="mui-table-view" style="border-bottom: solid 5px #dfdfdf;">
				<li class="mui-table-view-cell">
					<router-link to="/aboutUs">
						<a class="mui-navigate-right">
							关于我们
						</a>
					</router-link>
				</li>
				<li class="mui-table-view-cell">
					<router-link to="/userAgreement">
						<a class="mui-navigate-right">
							用户注册服务协议
						</a>
					</router-link>
				</li>
			</ul>	
			<ul class="mui-table-view about_banben">
				<li class="mui-table-view-cell mui-media">
					<a href="javascript:;">
						<img class="mui-media-object mui-pull-left" src="../../assets/img/my/about_logo.png">
						<div class="mui-media-body">
							应用名称
							<p class="mui-ellipsis">版本：<em>V1.21</em></p>
						</div>
					</a>
				</li>
			</ul>
			<div class="mui-content-padded btn_bc">
				<button type="button" class="mui-btn mui-btn-block active-btn-red">检测新版本</button>	
			</div>
		</div>	
	</div>
</template>
<script>
	import Vue from 'vue'
	import {
		Header
	} from 'mint-ui';
	import {
		Button
	} from 'mint-ui';
	import {
		mapMutations
	} from 'vuex'

	import hearder from '../public/header.vue'

	export default {
		name: 'about',
		components:{
	        hearder
		},
		data() {
			return {
				is_fixed: true,
				title:'关于'
			}
		},
		methods: {
			

		}
	}
</script>
