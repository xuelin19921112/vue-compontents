<!-- 询价单主体组件 -->
<template>
	<div id="inquiry_main" class="">
		<div class="mian_content">
			<div class="nothing" v-if="">
				<img src="../../assets/img/inquiry/dxj_no.png" />
				<p>您暂时没有任何评价信息</p>
				<button type="button" class="mui-btn mui-btn-success">立即前往添加</button>

			</div>
			<div class="inquiry-list">
				<div class="mui-card-header xj_header">
					<label>询价单号：<em>1826739286255</em></label>
					<span>2017-01-07<em>18:21:41</em></span>
				</div>
				<div class="mui-input-row mui-checkbox mui-left list_bd">
					<div class="Y_goods-lists-sub">
						<img src="../../assets/img/search/dxj_img.png">
						<div>
							<p>
								<a class="ovfh">迈氏 火花塞</a>
							</p>
							<p><span>L1KD872679F</span></p>
							<p>
								<span class="org">¥<em>98</em></span>
								<span class="mui-badge mui-badge-success span-color">品牌件</span>
							</p>
						</div>
					</div>
					<div class="Y_goods-lists-cd">
						<span><img src="../../assets/img/search/dxj_sincerity.png"/>北京物美公司</span>
						<span>产地：<em>中国</em></span>
					</div>
					<input name="checkbox" value="Item 1" type="checkbox" />
				</div>
				<div class="mui-input-row mui-checkbox mui-left list_bd">
					<div class="Y_goods-lists-sub">
						<img src="../../assets/img/search/dxj_img.png">
						<div>
							<p>
								<a class="ovfh">迈氏 火花塞</a>
							</p>
							<p><span>L1KD872679F</span></p>
							<p>
								<span class="org">¥<em>98</em></span>
								<span class="mui-badge mui-badge-success span-color">品牌件</span>
							</p>
						</div>
					</div>
					<div class="Y_goods-lists-cd">
						<span><img src="../../assets/img/search/dxj_sincerity.png"/>北京物美公司</span>
						<span>产地：<em>中国</em></span>
					</div>
					<input name="checkbox" value="Item 1" type="checkbox" />
				</div>
				<div class="mui-card-footer inquiry_footer">
					<button type="button" class="mui-btn mui-btn-outlined" @click="deleteAll()">删除询价单</button>
					<button type="button" class="mui-btn mui-btn-warning mui-btn-outlined" @click="tap()">立即下单</button>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	import Vue from 'vue'
	export default {
		name: 'inquiry_main',
		//首页一开始默认加载的数据
		created: function() {

		},
		data() {
			return {

			}
		},
		computed: {

		},
		methods: {
			tap: function() {
				mui.toast('您还没有选中配件…');
			},
			deleteAll: function() {
				var btnArray = [];
				mui.confirm('确认要删除已选中的配件？', btnArray, function(e) {
					if(e.index == 1) {
						//						info.innerText = '你刚确认MUI是个好框架';
					} else {
						//						info.innerText = 'MUI没有得到你的认可，继续加油'
					}
				})
			}
		}
	}
</script>
<style>
	#inquiry_main .mian_content{
		background: #fff;
	}
	#inquiry_main .nothing {
		font-size: 0.7rem;
		line-height: 3rem;
		text-align: center;
		padding-top: 2rem;
	}
	
	#inquiry_main .nothing img {
		width: 5.25rem;
		height: 5.25rem;
	}
	
	#inquiry_main .nothing .mui-btn-success {
		background: #7bb951;
		border: solid 1px #7bb951;
		padding: 0.6rem 2rem;
	}
	
	.xj_header {
		font-size: 0.6rem;
		border-top: 1px solid #dfdfdf;
	}
	
	.xj_header em {
		font-style: normal;
		margin-left: 4px;
	}
	
	.xj_header label em {
		color: #e25d2e;
	}
	
	#inquiry_main .Y_goods-lists-cd {
		border-bottom: none;
	}
	
	.inquiry_footer {
		background: #f9f9f9;
		min-height: 44px;
		padding: 10px 15px;
		width: auto;
		font-size: 0.6rem;
	}
	
	.inquiry_footer .mui-btn {
		width: auto;
		font-size: 0.6rem;
	}
	
	.inquiry_footer .mui-btn-warning {
		color: #e25d2e;
		border: 1px solid #e25d2e!important;
	}
	
	.inquiry_footer:before {
		height: 0px;
	}
	
	#inquiry_main .mui-checkbox.mui-left input[type=checkbox],
	#inquiry_main .mui-radio.mui-left input[type=radio] {
		left: 0.5rem!important;
	}
</style>