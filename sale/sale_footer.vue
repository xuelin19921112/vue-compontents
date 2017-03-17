<!-- 底部导航栏 -->
<template>
	<div id="footer" class="footer">
		<div class="footer_content">
			<ul >
				<li class="fl bdt text-center"><router-link to="/"><img src="../../assets/img/footer/footer_news.png" /><span>云采购</span></router-link></li>
				<li class="fl bdt text-center"><router-link to="/sale"><img src="../../assets/img/footer/footer_cloud_sales_click.png" /><span class="font-color-green">云销售</span></router-link></li>
				<li class="fl bdt text-center"><router-link to="/my"><img src="../../assets/img/footer/footer_me.png" /><span>我的</span></router-link></li>
			</ul>
		</div>
	</div>
</template>
<script>
import Vue from 'vue'

export default {
	name: 'footer',
	data () {
		return {
			
		}
	},
	methods : {
		
	},
	computed : {
	
	}
}
</script>

