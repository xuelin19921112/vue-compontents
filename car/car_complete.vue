<!--选车型完成选择组件-->
<template>
	<div id="car_complete" class="">
		<mt-header title="完成选择" :fixed="is_fixed" >
			<mt-button  slot="left">
				<a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
			</mt-button>
		</mt-header>
		<div class="complete-content">
			<p class="complete-content-title">
				<img src="../../assets/img/carchoose/success-green.png" />
				<span class="font-color-green"> 车型选择成功！</span>
			</p>
			<img src="../../assets/img/carchoose/xcx_car_successimg.png" class="car-band"/>
			<p style="line-height: 2rem;">北京奔驰 C级 C300 运动型 Grand Edition 3.0L</p>
			<p style="line-height: 1rem;">七档手自一体 W204 （2014）</p>
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
	name: 'car_complete',
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
	#car_complete {
		margin-bottom: 2rem;
	}
	#car_complete .mint-header{
		background-color: #fff;
		color: #000;
		border-bottom: solid 1px #dfdfdf;
	}
	#car_complete .complete-content{
		margin-top: 3rem;
		font-size: 0.65rem;
		text-align: center;
	}
	#car_complete .complete-content .complete-content-title{
		font-size: 0.9rem;
		line-height: 2.5rem;
	}
	#car_complete .complete-content .complete-content-title img{
		width: 1.25rem;
		height: 1.25rem;
		vertical-align: middle;
	}
	#car_complete .complete-content .car-band{
		width: 4rem;
		height: 4rem;
		margin: 1.5rem 0;
	}
</style>