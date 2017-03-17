<!-- 云销售报价单主体组件 -->
<template>
	<div id="quotation_main">
		<div class="mian_content">
			<ul class="buyer-info">
				<li class="font-color-red">买手：152***17</li>
				<li><span>LK132332452</span><span style="padding-left: 1.4rem;">前刹车片</span></li>
				<li><span>期望价:</span><span class="font-color-red font-small">￥</span><span class="font-color-red">198.00</span></li>
				<li><span>备注:</span><span>我只要博世原厂的刹车片，其他的不要。</span></li>
				<li class="buyer-info-account">x1</li>
			</ul>
			<div class="brand">
				<select style="border: solid 1px #ea7544!important">
					<option value="0">请选择品质</option>
					<option value="1">品牌件</option>
					<option value="2">正厂件</option>
					<option value="3">拆车件件</option>
				</select>
				<div class="mui-input-row brand-name" style="float: right;">
					<input type="text" placeholder="" value="迈氏">
				</div>
			</div>
			<div class="sale-price">
				<h5 class="font-color-red">销售价格<span class="font-color-grey"> (单价) </span></h5>
				<div class="mui-input-row brand-name">
					<input type="text" placeholder="请输入您的销售价格" value="" style="height: 1.2rem;">
				</div>
				<div class="mui-input-row mui-checkbox mui-left  charge">
					<input name="checkbox" value="" type="checkbox" checked="checked" @change="tapInput(checked)" >
					<label>含税价格</label>
				</div>
			</div>
			<div class="remark">
				<h5 class="font-color-red">备注</h5>
				<textarea placeholder="备注信息最多不超过100个字"></textarea>
			</div>
			<button type="button" class="mui-btn sendBtn btn-grey" @click="toast()">立即发送报价</button>
		</div>
	</div>
</template>

<script>
	export default {
		name: 'quotation_main',
		//首页一开始默认加载的数据
		created: function() {

		},
		components: {

		},
		data() {
			return {
				checked:[{
					state:false
				}]
			}
		},
		computed: {

		},
		methods: {
			tapInput:function(item){
				item.state = !item.state;
				console.log(item.state);
			},
			toast:function(){
				mui.toast('报价信息发送成功，请等待买方确认...');
			}
		}
	}
</script>
<style>
	#quotation_main .buyer-info {
		border-top: solid 1px #dfdfdf;
		padding: 0.5rem 0;
		border-bottom: solid 5px #dfdfdf;
		position: relative;
	}
	
	#quotation_main .buyer-info li {
		font-size: 0.65rem;
		line-height: 1.2rem;
		padding-left: 0.35rem;
	}
	
	#quotation_main .buyer-info .buyer-info-account {
		position: absolute;
		top: 3rem;
		right: 0.5rem;
	}
	
	#quotation_main .brand {
		overflow: hidden;
		padding: 0.8rem 0 0.8rem 0.35rem;
		border-bottom: solid 1px #dfdfdf;
	}
	
	#quotation_main .brand select {
		width: 5.5rem;
		text-align: center;
	}
	
	#quotation_main .brand-name {
		width: 9rem;
		margin-right: 0.5rem;
	}
	
	#quotation_main .brand-name input {
		border: none;
		font-size: 0.65rem;
		border-bottom: solid 1px #dfdfdf;
		margin: 0;
	}
	
	#quotation_main .sale-price,#quotation_main .remark {
		font-size: 0.65rem;
		padding-left: 0.35rem;
		padding-bottom: 0.5rem;
		position: relative;
		border-bottom: solid 1px #dfdfdf;
	}
	
	#quotation_main .sale-price h5,#quotation_main .remark h5 {
		font-size: 0.65rem;
		line-height: 1.3rem;
	}
	#quotation_main .sale-price .charge{
		position: absolute;
		top: 1rem;
		right: 0.5rem;
	}
	#quotation_main .remark textarea{
		width: 15rem;
		height: 3.4rem;
		background: #efefef;
		border: solid 1px #dfdfdf;
		border-radius: 0.2rem;
		margin: 0 0.2rem;
		font-size: 0.65rem;
	}
	#quotation_main .sendBtn{
		width: 15rem;
		height: 2rem;
		line-height: 1.2rem;
		margin: 1.25rem 0.5rem;
		color: #fff;
	}
</style>