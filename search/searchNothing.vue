<!-- 根据OE号查询无结果 -->
<template>
	<div v-show="show" id="search_nothing" >
    	<img src="../../assets/img/search/xcx_car_no.png" />
    	<p>抱歉，当前没有检索到相关数据</p>
    	<p>您可以试试搜索其他配件</p>
    </div>
</template>

<script>
import Vue from 'vue'
export default {
	name: 'search_nothing',
	//首页一开始默认加载的数据
	created : function(){
	
	},
	props:{
		show:{
			type:Boolean,
			default:false
		}
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
