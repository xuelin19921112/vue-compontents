<!-- 询价单组件 -->
<template>
	<div id="inquiry" class="app">
		
		<!-- 顶部 -->
        <inquiry-header></inquiry-header>
        
		<!-- 页面主体 -->
        <inquiry-main></inquiry-main>

	</div>
</template>

<script>
import Vue from 'vue'
import inquiryHeader from '../inquiry/inquiry_header.vue'
import inquiryMain from '../inquiry/inquiry_main.vue'

export default {
	name: 'useTo',
	components:{
        inquiryHeader,inquiryMain
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