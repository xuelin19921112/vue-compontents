<!-- 关于我们组件 -->
<template>
	<div id="about-us" class="">
		 <hearder :title="title"></hearder>
		<div class="about-us-content">
			<p style="text-align: center;">
				<span style="font-size:12px"><strong>北迈云经销商端--口袋里的汽配城</strong></span>
			</p>
			<p>
				<span style="font-size:12px"><strong>公司介绍</strong></span>
			</p>
			<p>
				<span style="font-size:12px;color:#3f3f3f">北迈科技成立于1998年，拥有8年的汽车后市场和16年电商运营经验，目前，公司已于全国中小企业股份转让系统（新三板）正式挂牌（股票简称：北迈科技，股票代码：836848） 。&nbsp;&nbsp;</span>
			</p>
			<p>
				<span style="font-size:12px;color:#3f3f3f">北迈科技旗下具有北迈云和北迈网两大支柱业务，以及同步支撑双业务发展的超大型零配件数据库系统，该系统覆盖80%以上主流车型，积累了汽车零配件标准SKU达350万余条，形成了独一无二的竞争力壁垒。其中，北迈云是一套开放式SaaS形态的汽车后市场业务流优化与交易撮合解决方案，借助北迈科技强大的数据引擎、自主知识产权的ERP系统、交易算法与物流算法等核心科技能够解决汽车零配件数据标准化、商品化等行业壁垒; 北迈网则是中国具有影响力的汽车零配件销售及快装服务（B2C&amp;O2O）平台之一，累计服务用户超过4000万频次，可一站式解决终端车主的汽车维修与保养需求。</span>
			</p>
			<p>
				<span style="font-size:12px"><strong>北迈云介绍</strong></span>
			</p>
			<p>
				<span style="font-size:12px"><strong>北迈云是一套开放式SaaS形态的汽车后市场业务流优化与交易撮合解决方案，它内置了北迈科技强大的数据引擎、自主知识产权的ERP系统、交易算法与物流算法等，致力于解决汽车零配件数据标准化、商品化等行业壁垒。目前，北迈云开通了制造商、经销商、电商、修理厂和保险公司五大端口，将通过数据底层撮合端口间的交易产生与生态化发展，并为之提供数据、交易工具、仓储和金融等多方位的服务。 在北迈云发布之初，它就获得京东的战略合作背书，以及100多家国内外制造商与品牌商入驻。</strong></span>
			</p>
			<p>
				<span style="font-size:12px"><strong>北迈云经销商端核心功能</strong></span>
			</p>
			<p>
				<span style="font-size:12px"><strong>对卖家</strong></span>
			</p>
			<p>
				<span style="font-size:12px;color:#3f3f3f">高性能ERP系统：基于BS结构，布置便捷, 升级方便; 本地与云管理一键切换，双保障确保数据安全。</span>
			</p>
			<p>
				<span style="font-size:12px;color:#3f3f3f">标准化商品数据：所有上传到北迈云的商品信息均由北迈数据中进行标准化处理，标准化后的配件商品，不仅其适配性一目了然，还能在所有北迈数据体系支持的零配件销售前端被查找和售卖。</span>
			</p>
			<p>
				<span style="font-size:12px;color:#3f3f3f">多销售出口对接：北迈网、北迈企业购（修理厂和企业）、北迈与京东及其他第三方合作平台，以及即将推出的保险公司端口，都是北迈云内商品的销售出口</span>
			</p>
			<p>
				<span style="font-size: small;"><strong>对买家</strong></span>
			</p>
			<p>
				<span style="font-size:12px;color:#3f3f3f">配件在线查价：数百万零配件价格在线可查，实时更新，覆盖全车系近200个主机厂。</span>
			</p>
			<p>
				<span style="font-size:12px;color:#3f3f3f">上游经销商在线匹配：上千家品牌商和一级分销商在线回复询价，按车型搜索即可找到</span>
			</p>
			<p>
				<span style="font-size:12px;color:#3f3f3f">配件安全交易保障：上市公司建设的交易平台，配件品质可追溯，资金安全有保障<br/></span>
			</p>
			<p>
				<span style="font-size:12px;color:#3f3f3f">口袋里的汽配城：网页与APP双平台，汽配城指尖可达</span>
			</p>
			<p>
				<span style="font-size:12px;color:#3f3f3f">北迈云可交易的商品数据</span>
			</p>
			<p>
				<span style="font-size:12px;color:#ff0000">截止2017年1月份，北迈云已经收录了共计438,1343条有库存、带价格，标准化的配件商品数据，这数量正以每天数千条的速度快速增加。（下表图所示是按车系和主机厂划分的配件商品数量）。</span>
			</p>
			<p>
				<span style="font-size:12px;color:#ff0000"><br/></span>
			</p>
		</div>
	</div>
</template>
<script>
import hearder from '../public/header.vue'
	import Vue from 'vue'
	import {
		Header
	} from 'mint-ui';
	import {
		Button
	} from 'mint-ui';
	import {
		mapMutations
	} from 'vuex'

	

	export default {
		name: 'about-us',
		components:{
	        hearder
		},
		data() {
			return {
				is_fixed: true,
				title:'关于我们'
			}
		},
		methods: {

		}
	}
</script>
