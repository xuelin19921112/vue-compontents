<!-- 适用车型顶部导航组件 -->
<template>
	<div id="useTo_header" class="">
		<mt-header title="一汽大众-宝来" :fixed="is_fixed" >
			<mt-button @click.native="refresh" slot="left">
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
	name: 'useTo_header',
	data () {
		return {
			is_fixed : true
		}
	},
	methods : {
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
	#useTo_header{
		margin-bottom: 2rem;
	}
	#useTo_header .mint-header{
		background-color: #fff;
		color: #000;
	}
</style>