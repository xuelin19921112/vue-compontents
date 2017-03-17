<!--选车型首页导航组件-->
<template>
	<div id="car_header" class="">
		<mt-header title="选车型" :fixed="is_fixed">
			<mt-button  slot="left">
				<a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
			</mt-button>
			<mt-button  slot="right">
				<img src="../../assets/img/carchoose/xcx_history.png" alt="" width="20" height="20">
			</mt-button>
		</mt-header>
		<div class="car-search">
			<input type="search" class="car-VIN" placeholder="请输入17位VIN码查询车型">
			<img src="../../assets/img/search/xcx_search.png" class="xcx_search" />
		</div>
	<!--车型索引表-->
		
		

	</div>
</template>

<script>
import Vue from 'vue'
import {Header} from 'mint-ui';
import {Button} from 'mint-ui';
import {mapMutations} from 'vuex'

//components("mt-header", Header);
//components("mt-button", Button);

export default {
	name : 'car_header',
	data () {
		return{
			is_fixed:true
		}
	},
	methods : {
		
	}
};

</script>


<script type="text/javascript" charset="utf-8">
	
</script>

<style>
	#car_header {
		/*margin-bottom: 2rem;*/
	}
	#car_header .mint-header{
		background-color: #fff;
		color: #000;
		border-bottom: solid 1px #dfdfdf;
	}
	#car_header .car-search{
		width: 15rem;
		height: 1.9rem;
		margin: 0.5rem auto;
		border:1px solid #dfdfdf;
		border-radius:0.1rem;
		overflow: hidden;
	}
	.car-search .car-VIN {
		width: 13rem;
		height: 1.9rem;
		line-height: 1.9rem;
		float: left;
		text-align: left;
		color: #777;
		background: #fff;
	}
	.car-search .xcx_search {
		width: 1.3rem;
		height: 1.2rem;
		float: right;
		margin: 0.3rem 0.35rem 0 0;
	}
	
</style>