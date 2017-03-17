<!-- 云销售报价单组件 -->
<template>
	<div id="quotation" class="app">
		<!-- 顶部 -->
        <quotation-hearder></quotation-hearder>
        
		<!-- 页面主体 -->
        <quotation-main></quotation-main>

	</div>
</template>

<script>
import Vue from 'vue'
import quotationHearder from '../quotation/quotation_header.vue'
import quotationMain from '../quotation/quotation_main.vue'

export default {
	name: 'quotation',
	components:{
        quotationHearder,quotationMain
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