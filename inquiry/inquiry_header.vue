<!-- 询价单顶部导航组件 -->
<template>
	<div id="inquiry_header" class="">
		<mt-header title="询价单" :fixed="is_fixed" >
			<mt-button  @click.native="refresh" slot="left">
				<a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
			</mt-button>
		</mt-header>
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
	name: 'inquiry_header',
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
	#inquiry_header {
		margin-bottom: 2rem;
	}
	#inquiry_header .mint-header{
		background-color: #fff;
		color: #000;
	}
</style>
