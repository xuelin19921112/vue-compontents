<!-- 待询价主体组件 -->
<template>
	<div id="pendingInquiry_main">
		<div class="mian_content">
			<div class="nothing" v-if="0">
				<img src="../../assets/img/inquiry/dxj_no.png" />
				<p>您暂时没有任何评价信息</p>
				<button type="button" class="mui-btn mui-btn-success">立即前往添加</button>
			</div>
			<div class="content-main">
				<div class="mui-input-row mui-checkbox mui-left bd_b">
					<input name="checkbox" value="Item 1" type="checkbox">
					<label>刹车片</label>
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
					<div class="mui-input-row input-num">
						<input type="text" placeholder="期望单价">
						<span>购买数量：</span>
						<div class="mui-numbox" data-numbox-min='1'>
							<button class="mui-btn mui-btn-numbox-minus btn-num-minus" type="button">-</button>
							<input class="mui-input-numbox" type="number"  value="1">
							<button class="mui-btn mui-btn-numbox-plus btn-num-plus" type="button">+</button>
						</div>
						<div class="mui-input-row">
							<textarea id="textarea" rows="1" placeholder="备注信息"></textarea>
						</div>
					</div>
				</div>
				<!--<footer class="fiexed-footer" v-if="">
					<div class="input-box">
						<div class="mui-input-row mui-checkbox mui-left">
							<input name="checkbox" value="" type="checkbox" class="qx">
							<label>全选</label>
						</div>
					</div>
					<div class="input-box">
						<div class="mui-input-row mui-checkbox mui-left">
							<input name="checkbox" value="true" type="checkbox" class="qx">
							<label>含税</label>
						</div>
					</div>
					<div class="inquiryBtn" style="float: right;">
						<button type="button" class="mui-btn mui-btn-danger" @click="tap()">
						发布询价
					</button>
					</div>
				</footer>-->
				<footer class="fiexed-footer">
					<div class="input-box">
						<div class="mui-input-row mui-checkbox mui-left">
							<input name="checkbox" value="" type="checkbox" class="qx">
							<label>全选</label>
						</div>
					</div>
					<div class="inquiryBtn" style="float: right;">
						<button type="button" class="mui-btn mui-btn-danger btn-grey" @click="deleteAll()">
						删除
					</button>
					</div>
				</footer>
			</div>

		</div>
	</div>
</template>

<script>
    import pendingInquiryHearder from 'components/pendingInquiry/pendingInquiry_header.vue'
	export default {
		name: 'pendingInquiry_main',
		//首页一开始默认加载的数据
		created: function() {

		},
		components: {
           pendingInquiryHearder
		},
		data() {
			return {

			}
		},
		computed: {

		},
		methods: {
			tap:function(){
				mui.toast('您还没有选中配件…');
			},
			deleteAll:function(){
				var btnArray = [];
				mui.confirm('确认要删除已选中的配件？', btnArray, function(e) {
					if (e.index == 1) {
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
	#pendingInquiry_main .nothing{
		font-size: 0.7rem;
		line-height: 3rem;
		text-align: center;
		padding-top: 2rem;
	}
	#pendingInquiry_main .nothing img{
		width: 5.25rem;
		height: 5.25rem;
	}
	#pendingInquiry_main .nothing .mui-btn-success{
		background: #7bb951;
		border: solid 1px #7bb951;
		padding: 0.6rem 2rem;
	}
	#pendingInquiry_main .content-main {
		background: #fff;
		padding-bottom: 2.5rem;
	}
	
	.Y_goods-lists-sub {
		min-height: 65px;
		padding: 0.3rem 0.3rem 0.3rem 2.2rem;
		color: #292929;
		overflow: hidden;
	}
	
	.Y_goods-lists-sub img {
		float: left;
		width: 4.68rem;
		height: 4.68rem;
		border: solid 1px #dfdfdf;
		;
	}
	
	.Y_goods-lists-sub>a {
		float: left;
		margin: auto;
		width: 4.68rem;
		height: 4.68rem;
		overflow: hidden;
		border: 1px solid #cccccc;
	}
	
	.Y_goods-lists-sub div {
		padding: 0 10px 0 5.2rem;
	}
	
	.Y_goods-lists-sub div p {
		width: 90%;
		margin-bottom: 0;
		font-size: 0.45rem;
		line-height: 1rem;
	}
	
	.Y_goods-lists-sub div p a {
		color: #151515;
		font-size: 0.63rem;
	}
	
	.Y_goods-lists-sub div p span {
		font-size: 0.45rem;
	}
	
	.Y_goods-lists-sub div p span.org,
	.Y_goods-lists-sub div p span.org em {
		color: #ea7544;
		font-style: normal;
	}
	
	.Y_goods-lists-sub div p span.org em {
		font-size: 0.63rem;
	}
	
	.Y_goods-lists-sub div p span.span-color {
		background: #7bb951;
	}
	
	.Y_goods-lists-sub div p:last-child {
		margin-top: 0.5rem;
	}
	
	.bd_b {
		border-bottom: 1px solid #dfdfdf;
	}
	
	.list_bd {
		border-bottom: 1px solid #dfdfdf;
	}
	
	.list_bd>input {
		top: 30px!important;
	}
	
	.Y_goods-lists-cd {
		font-size: 0.55rem;
		font-style: normal;
		padding: 0.3rem 0.35rem 0.3rem 2.2rem;
		border-bottom: 1px dashed #dfdfdf;
	}
	
	.Y_goods-lists-cd span:first-child {
		display: inline-block;
	}
	
	.Y_goods-lists-cd span:first-child img {
		height: 0.9rem;
		vertical-align: middle;
		margin-right: 4px;
	}
	
	.Y_goods-lists-cd span:last-child {
		float: right;
	}
	
	.Y_goods-lists-cd span em {
		font-style: normal;
	}
	
	.input-num {
		padding: 0.65rem 10px 0.65rem 2.2rem;
	}
	
	.input-num input {
		width: 45%;
		height: 1.75rem;
		border-radius: 0;
		font-size: 0.6rem;
	}
	
	.input-num span {
		font-size: 0.45rem;
		padding-left: 0.2rem;
	}
	
	.input-num .mui-numbox {
		width: 29%;
		margin: 2px 0;
		height: 1.5rem;
	}
	
	#pendingInquiry_main .btn-num-minus,
	#pendingInquiry_main .btn-num-plus {
		border-radius: 1.25rem;
		width: 1rem;
		height: 1rem;
		background-color: #777;
		color: #fff;
		top: 0.25rem;
	}
	
	.input-num .mui-numbox {
		border: none;
		padding: 0.9px;
		background: #fff;
	}
	
	.input-num .mui-numbox .mui-input-numbox {
		border-left: none!important;
		border-right: none!important;
	}
	
	.input-num .mui-input-row {
		height: 1.75rem;
		line-height: 0.6rem;
	}
	
	.input-num .mui-input-row textarea {
		height: 1.75rem;
		line-height: 0.85rem;
		overflow: hidden;
		margin-bottom: 0;
		font-size: 0.6rem;
	}
	
	.mui-checkbox input[type=checkbox]:checked:before,
	.mui-radio input[type=radio]:checked:before {
		color: #ea7544;
	}
	
	#pendingInquiry_main .fiexed-footer {
		width: 100%;
		height: 2.5rem;
		position: fixed;
		left: 0;
		bottom: 0;
		border-top: solid 1px #dfdfdf;
		background: #fff;
	}
	
	#pendingInquiry_main .mui-checkbox.mui-left label,
	.mui-radio.mui-left label {
		padding-left: 2.2rem;
		padding-right: 0;
	}
	
	#pendingInquiry_main .input-box {
		float: left;
		width: 4.5rem;
		height: 100%;
		margin-top: 0.3rem;
	}
	
	#pendingInquiry_main .fiexed-footer .hs {}
	
	#pendingInquiry_main .mui-checkbox.mui-left input[type=checkbox],
	#pendingInquiry_main .mui-radio.mui-left input[type=radio] {
		left: 0.5rem!important;
	}
	
	#pendingInquiry_main .mui-btn-danger {
		padding: 0.9rem 1.7rem;
		border-radius: 0;
	}
	/*#pendingInquiry_main .fiexed-footer .bac-color{
		background: #ea4444;
		border: solid 1px #ea4444;
	}*/
</style>