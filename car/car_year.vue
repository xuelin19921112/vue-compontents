<!--请选择车款组件-->
<template>
	<div id="car_year" class="">
		<mt-header title="请选择年款" :fixed="is_fixed" class="">
			<!--<router-link to="##" slot="left">
				<mt-button icon="more" @click="showSlideFn"></mt-button>
			</router-link>-->
			<mt-button @click.native="refresh" slot="left">
				<img src="../../assets/img/carchoose/dxj_return.png" width="20" height="20">
					<!--<a id="icon-chatbubble">
						<span class="mui-icon mui-icon-chatbubble"></span>
					</a>-->
			</mt-button>
		</mt-header>
		
<template>
	
<script>
import Vue from 'vue'
import {Header} from 'mint-ui';
import {Button} from 'mint-ui';
import {mapMutations} from 'vuex'

components("mt-header", Header);
components("mt-button", Button);

export default {
	name : 'car_year',
	//一开始默认加载的数据
	data () {
		return{
			is_fixed:true
		}
	},
	methods : {
		...mapMutations([
			'shouSlideFn',
		]),
		refresh() {
			this.$store.commit({
				type : 'changeRefreshName',
				refreshName : this.$route.path
			})
		}
	}
};	
</script>