<!-- 待询价单顶部导航组件 -->
<template>
	<div id="pendingInquiry_header" class="">
		<mt-header title="待询价" :fixed="is_fixed" >
			<mt-button  slot="left">
				<a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
			</mt-button>
			<mt-button  slot="right">
				<span @click="edit">{{IsEdit?'完成':'编辑'}}</span>
			</mt-button>
		</mt-header>
	</div>
</template>

<script>

export default {
	name: 'pendingInquiry_header',
	props:{
		value:{
			type:Boolean,
			default:false
		}
	},
	data () {
		return {
			is_fixed : true,
			IsEdit:false
		}
	},
	methods : {
		refresh(){
			this.$store.commit({
				type : 'changeRefreshName',
				refreshName : this.$route.path
			})
			//window.location.reload()
		},
		edit(){
			this.IsEdit=!this.IsEdit;
			this.$emit('edit',this.IsEdit);
			
		}
	}
}
</script>

<style>
	#pendingInquiry_header {
		margin-bottom: 2rem;
	}
	#pendingInquiry_header .mint-header{
		background-color: #fff;
		color: #000;
		border-bottom: solid 1px #dfdfdf;
	}
</style>
