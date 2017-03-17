<!--VIN查询组件-->
	<template>
	<div id="car_VIN" class="">
		<mt-header title="VIN查询" :fixed="is_fixed" class="car-VIN-title">
			<mt-button  slot="left">
				<a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
			</mt-button>
		</mt-header>
		<div class="car-VIN-search">
			<input type="search" class="car-part-VIN" placeholder="请输入OE号或配件名称查询">
			<img src="../../assets/img/carchoose/xcx_search.png" class="xcx_search" />
		</div>
		
		<!--VIN查询无结果-->
		<div class="car-nVIN-result">
			<img src="../../assets/img/search/xcx_car_no.png">
			<p>当前没有查询到相关车型信息</p>
			<p>请检查VIN是否正确或使用品牌选车</p>
		</div>
		
		<!--VIN查询结果列表-->
		<div class="car-hVIN-result">
			<ul class="mui-table-view">
    			<li class="mui-table-view-cell">
        			<a class="mui-navigate-right">北汽幻速 2012-2016</a>
    				<img src="../../assets/img/carchoose/xcx_car2.png" >
    			</li>
    			<li class="mui-table-view-cell">
        			<a class="mui-navigate-right">北汽幻速 2012-2016 北汽0.3L 手自一体FdssdGkanb 手动W (2016)</a>
    				<img src="../../assets/img/carchoose/xcx_car2.png">
    			</li>  
			</ul>
		</div>                                                                                                                                                                                         
	</div>
</template>

<script>
import Vue from 'vue'
import {Header} from 'mint-ui';
import {Button} from 'mint-ui';

//components("mt-header", Header);
//components("mt-button", Button);

export default {
	name : 'car_VIN',
	data () {
		return{
			is_fixed:true
		}
	},
	methods : {
		
	}
};

</script>
<style>
	.car-VIN-title{
		color:#151515;
		background-color:#ffffff;
		border-bottom:1px solid #dfdfdf;
	}
	.car-VIN-search{
		width: 14rem;
		height: 2rem;
		border:1px solid #dfdfdf;
		margin:2.5rem 1rem 0 1rem;
		border-radius:0.1rem;
		overflow:hidden;
	}
	.car-VIN-search .car-part-VIN {
		width: 12rem;
		height: 1.95rem;
		line-height: 1.95rem;
		color: #777;
		background-color: #fff;
	}
	.car-VIN-search .xcx_search {
		width: 1.3rem;
		height: 1.2rem;
		vertical-align: middle;
	}
	.car-nVIN-result {
		text-align: center;
	}
	.car-nVIN-result img{
		width: 5.25rem;
		height: 5.25rem;
		margin: 2.5rem 0 1rem;
	}
	.car-hVIN-result{
		margin-top: 1rem;
	}
	.car-hVIN-result .mui-table-view:before{
		height: 0;
	}
	.car-hVIN-result .mui-table-view-cell>a:not(.mui-btn){
		width: 15.2rem;
		padding-left: 2.9rem;
		padding-right: 1.2rem;
		white-space:pre-wrap;
		font-size: 0.7rem;
	}
	.car-hVIN-result img{
		width: 1.75rem;
		height: 1.75rem;
		position: absolute;
		top: 0.35rem;
		left: 0.875;
	}
	.car-hVIN-result .mui-table-view-cell{
		position: relative;
	}
	#car_VIN input[type=search]{
		text-align: left;
	}
</style>