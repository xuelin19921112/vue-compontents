<!-- 选车型主体组件 -->
<template>
	<div id="car_main">
		<div class="mian_content">
			<mt-index-list>
				<mt-index-section index="A">
					<router-link to="/carSeries"><mt-cell title="Aaron"></mt-cell></router-link>
					<mt-cell title="Alden"></mt-cell>
					<mt-cell title="Austin"></mt-cell>
				</mt-index-section>
				<mt-index-section index="B">
					<mt-cell title="Baldwin"></mt-cell>
					<mt-cell title="Braden"></mt-cell>
				</mt-index-section>
				<mt-index-section index="C">
					<mt-cell title="Caldwin"></mt-cell>
					<mt-cell title="Craden"></mt-cell>
				</mt-index-section>
				<mt-index-section index="Z">
					<mt-cell title="Zack"></mt-cell>
					<mt-cell title="Zane"></mt-cell>
				</mt-index-section>
			</mt-index-list>
		</div>
	</div>
</template>

<script>
import {IndexList, IndexSection} from 'mint-ui';
import { mapGetters } from 'vuex'
import api from '../../api/urls.js'
import {MD5,Trim,CheckUserName,getcurrDateTime} from '../../content/utils.js';
	export default {
		name: 'car_main',
		//首页一开始默认加载的数据
		created: function() {

		},
		components: {},
		data() {
			return {}
		},
		computed : {
	      
		},
		mounted(){
	       
		},
		methods: {
		}
	}
</script>

<style>
	#car_main .mint-cell-text::before{
		content: ' ';
		background: url(../../assets/img/carchoose/xcx_car1.png) 0 0 no-repeat;
		background-size: 100%;
		display: inline-block;
		width: 1.75rem;
		height: 1.75rem;
		vertical-align: middle;
		margin-right: 0.5rem;
	}
	#car_main .mint-indexsection-index{
		background: #fff;
	}
	#car_main .mint-indexlist-nav{
		border: none;
	}
</style>