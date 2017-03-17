<!-- 找配件组件 -->
<template>
	<div id="search" class="app">
		
		<div v-show="isshow" >
			<!-- 顶部 -->
			<!--<searchhearder></searchhearder>-->
			
			<!-- 页面主体 -->
			<searchmain></searchmain>
		</div>
	</div>
</template>

<script>
import Vue from 'vue'
import searchmain from '../search/search_main.vue'

export default {
	name: 'search',
	components:{
        searchmain
	},
	data () {
		return {
			isshow:true,
			scrolldistance:0
		}
	},
	// beforeRouteUpdate (to, from, next) {
    //    console.log(to,from);
	// 	if(to.name=="search")
	// 	{
    //       this.isshow=true;
	// 	  $("html,body").animate({scrollTop:this.scrolldistance},100);
	// 	}else
	// 	{
    //       this.isshow=false;
	// 	  this.scrolldistance=document.body.scrollTop;
	// 	}
	// 	next();
	// },
	methods : {
		
	}
}
</script>

<style>
	
</style>
