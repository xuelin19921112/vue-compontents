<!--请选择配置（第二步）组件-->
<template>
	<div id="car_sconfig" class="">
		<mt-header title="请选择配置" :fixed="is_fixed" >
			<mt-button  slot="left">
				<a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
			</mt-button>
		</mt-header>
		<div class="sconfig-content">
			<p class="sconfig-content-title">
				<span class="font-color-green">北京奔驰 > C级 > 第三代C级（2007-2014）> 2014年生产 > C300</span>
			</p>
			<ul class="mui-table-view fconfig-content-items">
				<li class="mui-table-view-cell">
					<router-link to="/carComplete">
						<a class="mui-navigate-right">
							3.0L 七档手自一体 运动型 Grand Edition
						</a>
					</router-link>
				</li>
			</ul>
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
	name: 'car_sconfig',
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
	#car_sconfig {
		margin-bottom: 2rem;
	}
	#car_sconfig .mint-header{
		background-color: #fff;
		color: #000;
		border-bottom: solid 1px #dfdfdf;
	}
	#car_sconfig .sconfig-content{
		margin-top: 2rem;
		font-size: 0.65rem;
	}
	#car_sconfig .sconfig-content .sconfig-content-title{
		line-height: 1.2rem;
		padding-left: 0.35rem;
		border-bottom: solid 5px #dfdfdf;
	}
	#car_sconfig .mui-table-view:before{
		height: 0;
	}
	#car_sconfig .mui-table-view-cell:after{
		left: 0;
	}
</style>