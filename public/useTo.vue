<!-- 适用车型详情组件 -->
<template>
	<div id="useTo" class="app">
		
		<!-- 顶部 -->
        <use-to-header></use-to-header>
        
		<!-- 页面主体 -->
        <use-to-main></use-to-main>

	</div>
</template>

<script>
import Vue from 'vue'
import useToHeader from '../useTo/useTo_header.vue'
import useToMain from '../useTo/useTo_main.vue'

export default {
	name: 'useTo',
	components:{
        useToHeader,useToMain
	},
	data () {
		return {
			
		}
	},
	methods : {
		
	}
}
</script>

<style>
	
</style>