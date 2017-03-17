<!--车系选择-->
<template>
	<div id="car_series" class="car-series-content">
		<mt-header title="请选择车系" :fixed="is_fixed" class="car-title car-line">
			<mt-button  slot="left">
				<a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
			</mt-button>
		</mt-header>
		<div class="car-lb">
			<img class="car-logo" src="../../assets/img/carchoose/xcx_car2.png">
			<span class="car-band">丰田</span>
		</div>
		<div class="car-series-name">
			<p class="car-cata">一汽丰田</p>
			<ul class="mui-table-view car-series-name">
    			<li class="mui-table-view-cell">
        			<router-link to="/carByear"><a class="mui-navigate-right">花冠</a></router-link>
    			</li>
    			<li class="mui-table-view-cell">
        			<a class="mui-navigate-right">卡罗拉</a>
    			</li>
			</ul>
		</div>
		<div class="car-series-name car-series-b">
			<p class="car-cata">进口丰田</p>
			<ul class="mui-table-view car-series-name">
    			<li class="mui-table-view-cell">
        			<a class="mui-navigate-right car-series-t">兰德路泽(陆地巡洋舰)</a>
    			</li>
    			<li class="mui-table-view-cell">
        			<a class="mui-navigate-right">海狮</a>
    			</li>
    			<li class="mui-table-view-cell">
        			<a class="mui-navigate-right">汉兰达</a>
    			</li>
    			<li class="mui-table-view-cell">
        			<a class="mui-navigate-right">普瑞维亚(大霸王)</a>
    			</li>
    			<li class="mui-table-view-cell">
        			<a class="mui-navigate-right">小霸王(WISH)</a>
    			</li>
    			<li class="mui-table-view-cell">
        			<a class="mui-navigate-right">普拉多</a>
    			</li>
    			<li class="mui-table-view-cell">
        			<a class="mui-navigate-right">丰田86</a>
    			</li>
    			<li class="mui-table-view-cell car-series-b">
        			<a class="mui-navigate-right">坦途</a>
    			</li>
			</ul>
		</div>
		<div class="car-series-empty"></div>
	</div>
</template>

<script>
import Vue from 'vue'
import {Header} from 'mint-ui';
import {Button} from 'mint-ui';
import {mapMutations} from 'vuex'

components("mt-header", Header);
components("mt-button", Button);

export default {
	name : 'car_series',
	//首页一开始默认加载的数据
	data () {
		return{
			is_fixed:true
		}
	},
	methods : {
		
	}
};	
</script>

<script>
	
</script>

<style>
	#car_series{
		margin-bottom: 2rem;
	}
	#car_series .mint-header{
		background-color: #fff;
		color: #000;
		border-bottom: solid 1px #dfdfdf;
	}
	.car-lb{
		font-size:0.75rem;
		line-height: 2.6rem;
		padding-left: 0.35rem;
		height: 2.6rem;
		overflow:hidden;
	}
	.car-logo{
		width:1.75rem;
		height:1.75rem;
		vertical-align: middle;
	}
	.car-series-name .car-cata{
		width: 100%;
		height:2rem;
		line-height:2rem;
		color:#4b9817;
		font-size:0.6rem;
		background-color: #efefef;
		padding-left:0.35rem;
	}
	.car-series-empty{
		height:0.825rem;
	}
</style>