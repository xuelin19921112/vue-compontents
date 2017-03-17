<!--请选择车款（小年款）组件-->
<template>
	<div id="car_syear" class="">
		<mt-header title="请选择年款" :fixed="is_fixed" >
			<mt-button  slot="left">
				<a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
			</mt-button>
		</mt-header>
		<div class="byear-content">
			<p class="byear-content-title">
				<span class="font-color-green">北京奔驰 > C级 > 第三代C级（2007-2014）</span>
			</p>
			<ul class="mui-table-view byear-content-items">
				<li class="mui-table-view-cell">
					<router-link to="/carFconfig">
						<a class="mui-navigate-right">
							2014年生产
						</a>
					</router-link>	
				</li>
				<li class="mui-table-view-cell">
					<a class="mui-navigate-right">
						2013年生产
					</a>
				</li>
				<li class="mui-table-view-cell">
					<a class="mui-navigate-right">
						2012年生产
					</a>
				</li>
				<li class="mui-table-view-cell">
					<a class="mui-navigate-right">
						2011年生产
					</a>
				</li>
				<li class="mui-table-view-cell">
					<a class="mui-navigate-right">
						2010年生产
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
	name: 'car_syear',
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
	#car_syear {
		margin-bottom: 2rem;
	}
	#car_syear .mint-header{
		background-color: #fff;
		color: #000;
		border-bottom: solid 1px #dfdfdf;
	}
	#car_syear .byear-content{
		font-size: 0.75rem;
		margin-top: 2rem;
	}
	#car_syear .byear-content .byear-content-title{
		line-height: 2.6rem;
		height: 2.6rem;
		padding-left: 0.35rem;
		border-bottom: solid 5px #dfdfdf;
	}
	#car_syear .mui-table-view:before{
		height: 0;
	}
	#car_syear .mui-table-view-cell:after{
		left: 0;
	}
</style>