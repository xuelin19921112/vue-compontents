<!--请选择配置（第一步）组件-->
<template>
	<div id="car_fconfig" class="">
		<mt-header title="请选择配置" :fixed="is_fixed" >
			<mt-button  slot="left">
				<a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
			</mt-button>
		</mt-header>
		<div class="fconfig-content">
			<p class="fconfig-content-title">
				<span class="font-color-green">北京奔驰 > C级 > 第三代C级（2007-2014）> 2014年生产</span>
			</p>
			<ul class="mui-table-view fconfig-content-items">
				<li class="mui-table-view-cell">
					<router-link to="/carSconfig">
						<a class="mui-navigate-right">
							C300
						</a>
					</router-link>	
				</li>
				<li class="mui-table-view-cell">
					<a class="mui-navigate-right">
						C260
					</a>
				</li>
				<li class="mui-table-view-cell">
					<a class="mui-navigate-right">
						C180
					</a>
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
	name: 'car_fconfig',
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
	#car_fconfig {
		margin-bottom: 2rem;
	}
	#car_fconfig .mint-header{
		background-color: #fff;
		color: #000;
		border-bottom: solid 1px #dfdfdf;
	}
	#car_fconfig .fconfig-content{
		margin-top: 2rem;
		font-size: 0.65rem;
	}
	#car_fconfig .fconfig-content .fconfig-content-title{
		line-height: 1.2rem;
		padding-left: 0.35rem;
		border-bottom: solid 5px #dfdfdf;
	}
	#car_fconfig .mui-table-view:before{
		height: 0;
	}
	#car_fconfig .mui-table-view-cell:after{
		left: 0;
	}
</style>