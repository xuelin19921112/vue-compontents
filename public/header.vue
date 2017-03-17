<!-- 顶部导航组件 -->
<template>
	<div id="header" class="">
		<mt-header :title="title" :fixed="is_fixed" >
			<mt-button  slot="left">
				<a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
			</mt-button>
			<mt-button slot="right">
				<img src="../../assets/img/home/cloud_procurement_news.png" alt="" width="20" height="20">
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
	name: 'header',
	props: {
		show: {
			type: Boolean,
			default: true
		},
		title:{
			type:String,
			default:" "
		}
	},
	data () {
		return {
			is_fixed : true
		}
	},
	methods : {
		
		
	}
}
</script>

<style>
	
</style>