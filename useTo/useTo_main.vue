<!-- 适用车型主体组件 -->
<template>
	<div id="useTo_main" class="">
		<div class="mian_content">
			<ul class="useTo-items">
				<li class="useTo-item-content">
					<p class="item-title">老宝来 1.6L 五挡手动（2001-2006）</p>
					<p class="item-use">用于：前刹车盘直径为280mm厚度为22mm的车型</p>
				</li>
				<li class="useTo-item-content">
					<p class="item-title">老宝来 1.6L 五挡手动（2001-2006）</p>
					<p class="item-use">用于：前刹车盘直径为280mm厚度为22mm的车型</p>
				</li>
			</ul>
		</div>
	</div>
</template>

<script>
import Vue from 'vue'
export default {
	name: 'useTo_main',
	//首页一开始默认加载的数据
	created : function(){
		
	},
	data () {
		return {
			
		}
	},
	computed : {

	},
	method : {
		
	}
}

</script>
<style>
	#useTo_main .useTo-items{
	}
	#useTo_main .useTo-items li{
		width: 100%;
		padding: 0.4rem;
		border-top: solid 1px #dfdfdf;
	}
	#useTo_main .useTo-items p{
		margin: 0;
	}
	#useTo_main .item-title{
		font-size: 0.75rem;
		line-height: 1rem;
	}
	#useTo_main .item-use{
		font-size: 0.65rem;
		line-height: 0.8rem;
		color: #e25d2e;
	}
</style>
