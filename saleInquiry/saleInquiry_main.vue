<!-- 云销售询价单主体组件 -->
<template>
	<div id="saleInquiry_main">
		<div class="mian_content">
			<ul class="inquiry-titles bdt bdb" id="inquiry-titles">
				<li class="inquiry-title-item fl text-center" v-for="item in list">
					<span v-text="item.name" :class="{active:item.iscur}" @click="setActive(item)"></span>
				</li>
			</ul>
			<div class="inquiry-content">
            <saleInquiryList></saleInquiryList>
				<!--<div class="inquiry-content-items" v-if="index==0">
					<ul>
						<li class="item-content">
							<div class="title">
								<span class="buyer">买手：111111</span>
								<span class="states font-color-red">待报价</span>
							</div>
							<div class="content">
								<ul class="content-left">
									<li><span>LK132332452</span><span style="padding-left: 1.4rem;">博世  前刹车片</span></li>
									<li><span>期望价：</span><span class="font-small font-color-red">￥</span><span class="font-color-red">198.00</span><span class="font-color-grey"> (含税) </span></li>
									<li><span>备注：</span><span style="padding-left: 0.2rem;">我只要博世原厂的刹车片，其他的不要。</span></li>
								</ul>
								<ul class="content-right">
									<li><span class="mui-badge mui-badge-success">品牌件</span></li>
									<li><span>x2</span></li>
								</ul>
							</div>
							<div class="time">
								<span class="nowtime font-color-grey">2017-01-18  18:21:41</span>
								<router-link to="/quotation">
									<button type="button" class="mui-btn mui-btn-danger offer">报价</button>
								</router-link>
							</div>
						</li>
					</ul>
				</div>
				<div class="inquiry-content-items" v-if="index == 1">
					<ul>
						<li class="item-content">
							<div class="title">
								<span class="buyer">买手：111111</span>
								<span class="states font-color-red">已报价</span>
							</div>
							<div class="content">
								<ul class="content-left">
									<li><span>LK132332452</span><span style="padding-left: 1.4rem;">博世  前刹车片</span></li>
									<li><span>期望价：</span><span class="font-small font-color-red">￥</span><span class="font-color-red">198.00</span><span class="font-color-grey"> (含税) </span></li>
									<li><span>备注：</span><span style="padding-left: 0.2rem;">我只要博世原厂的刹车片，其他的不要。</span></li>
								</ul>
								<ul class="content-right">
									<li><span class="mui-badge mui-badge-success">品牌件</span></li>
									<li><span>x2</span></li>
								</ul>
							</div>
							<div class="time">
								<span class="nowtime font-color-grey">2017-01-18  18:21:41</span>
								<span class="sale-price">销售价：<span class="font-color-red font-small">￥</span><span class="font-color-red">208.00</span><span class="font-color-grey"> (含税) </span></span>
							</div>
						</li>
					</ul>
				</div>
				<div class="inquiry-content-items" v-if="index == 2">
					<ul>
						<li class="item-content">
							<div class="title">
								<span class="buyer">买手：111111</span>
								<span class="states font-color-red">已报价</span>
							</div>
							<div class="content">
								<ul class="content-left">
									<li><span>LK132332452</span><span style="padding-left: 1.4rem;">博世  前刹车片</span></li>
									<li><span>期望价：</span><span class="font-small font-color-red">￥</span><span class="font-color-red">198.00</span><span class="font-color-grey"> (含税) </span></li>
									<li><span>备注：</span><span style="padding-left: 0.2rem;">我只要博世原厂的刹车片，其他的不要。</span></li>
								</ul>
								<ul class="content-right">
									<li><span class="mui-badge mui-badge-success">品牌件</span></li>
									<li><span>x2</span></li>
								</ul>
							</div>
							<div class="time">
								<span class="nowtime font-color-grey">2017-01-18  18:21:41</span>
							</div>
						</li>
					</ul>
				</div>-->
			</div>
		</div>

	</div>
</template>

<script>
import saleInquiryList from './saleInquiry_list.vue'

	export default {
		name: '',
		components: {
           saleInquiryList
		},
		data() {
			return {
				nothingshow: true,
				listshow: true,
				list: [{
					index: 0,
					name: '待报价',
					iscur: true
				}, {
					index: 1,
					name: '已报价',
					iscur: false
				}, {
					index: 2,
					name: '已过期',
					iscur: false
				}],
				index: 0
			}
		},
		computed: {

		},
		methods: {
			setActive: function(item) {
				this.list.forEach(x => {
					x.iscur = false;
				});
				item.iscur = true;
				this.index = item.index;
				console.log(item.index);
			}
		}
	}
</script>
