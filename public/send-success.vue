<!-- 发送成功组件 -->
<template>
	<div id="send-success" class="">
		<mt-header title="发送成功" :fixed="is_fixed" >
			<mt-button  slot="left">
					<a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
			</mt-button>
		</mt-header>
		<div class="success-content">
			<img src="../../assets/img/search/dxj_success.png" />
			<p>您的询价信息已经发送成功</p>
			<p>请耐心等待经销商的回复</p>
			<div class="group-btn">
				<span class="mui-btn mui-btn-success">
					前往询价单查看
				</span>
				<span class="mui-btn mui-btn-danger">
					继续询价
				</span>
			</div>
		</div>
	</div>
</template>
<script>
import Vue from 'vue'
import { Header } from 'mint-ui';
import { Button } from 'mint-ui';
import { mapMutations } from 'vuex'

Vue.component('mt-button', Button);
Vue.component('mt-header', Header);

export default {
	name: 'send-success',
	data () {
		return {
			is_fixed : true
		}
	},
	methods : {
		//映射点击事件为commit同名的mutations
		...mapMutations([
			'showSlideFn',
		]),
		refresh(){
			this.$store.commit({
				type : 'changeRefreshName',
				refreshName : this.$route.path
			})
			//window.location.reload()
		}
		
	}
}
</script>
<style>
	#send-success {
		margin-bottom: 2rem;
	}
	#send-success .mint-header{
		background-color: #fff;
		color: #000;
	}
	#send-success .success-content{
		margin-top: 2rem;
		border-top: solid 1px #dfdfdf;
		padding-top: 1.75rem;
		text-align: center;
		font-size: 0.7rem;
		line-height: 1rem;
	}
	#send-success .success-content p{
		margin-bottom: 0;
		color: #666;
	}
	#send-success .success-content img{
		width: 5.25rem;
		height: 5.25rem;
	}
	#send-success .group-btn{
		margin-top: 2.5rem;
	}
	#send-success .mui-btn-success{
		background-color: #7bb951;
		border: solid 1px #7bb951;
		padding: 0.5rem 1.5rem;
		margin-right: 0.8rem;
	}
	#send-success .mui-btn-danger{
		background-color: #ea7544;
		border: solid 1px #ea7544;
		padding: 0.5rem 1.5rem;
	}
</style>