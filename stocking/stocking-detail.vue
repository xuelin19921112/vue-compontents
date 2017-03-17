<!-- 备货详情组件 -->
<template>
	<div id="stocking-detail" class="">
		<mt-header title="备货详情" :fixed="is_fixed">
			<mt-button slot="left">
				<a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
			</mt-button>
		</mt-header>
		<div class="detail-content">
			<ul class="buyer-info">
				<p style="padding-left: 0.35rem;">备货号：<span>{{detail.PO_NUM}}</span></p>
				<li v-for="item in detail.Po_Deatail">
					<p><span>{{item.PROD_SKU}}</span><span style="padding-left: 1.4rem;" >{{item.ProductBrand_Name?item.ProductBrand_Name:''}} {{item.PROD_NAME}}</span></p>
					<p><span>单价:</span><span class="font-color-red font-small">￥</span><span class="font-color-red">{{item.IN_UNIT_PRICE}}</span></p>
					<p  class="buyer-info-account">{{"x"+item.PROD_COUNT}}</p>
				</li>
			</ul>
			<ul class="detail-select" >
				<li>
					<div class="mui-input-row mui-checkbox mui-left">
						<input name="checkbox" value="" type="checkbox" class="qx" v-model="IsDelivery">
						<label>送货</label>
					</div>
				</li>
				<li>
					<div class="mui-input-row mui-checkbox mui-left">
						<input name="checkbox" value="" type="checkbox" class="qx" v-model="IsTax">
						<label>含税</label>
					</div>
				</li>
				<li>
					<div class="mui-input-row mui-checkbox mui-left">
						<input name="checkbox" value="" type="checkbox" class="qx"  v-model="HasGoods">
						<label>带包装</label>
					</div>
				</li>
			</ul>
			<div class="group-btn" v-if='status==1'>
				<button type="button" class="mui-btn nogoods" @click='AuditPopOrder2'>无货</button>
				<button type="button" class="mui-btn stockingbtn" @click='AuditPopOrder'>备货完成</button>
			</div>
		</div>
	</div>
</template>
<script>
	import Vue from 'vue'
	import { Header } from 'mint-ui';
	import { Button } from 'mint-ui';
	import { mapMutations} from 'vuex'
	import { mapActions, mapGetters } from 'vuex'
    import { Toast } from 'mint-ui';
    import api from 'src/api/urls.js'
	
	export default {
		name: 'stocking-detail',
		created: function() {
		   
		},
		data() {
			return {
				is_fixed: true,
				loading:false,
				IsDelivery:false,
               	IsTax:false,
               	HasGoods:false,
                status:1,
                detail:{}
			}
		},
		 beforeRouteEnter(to, from, next) {
	        next(vm => {
	            vm.detail=vm.$route.params.detail;
	            vm.status=vm.$route.params.status;
	        })
	    },

		computed: {
            ...mapGetters(
                {userinfo:'UserInfo'}
            )
		},
		mounted(){
            this.$nextTick(()=>{
             	
            })
	    },
		methods: {
			AuditPopOrder(){
                let _this=this;
                var param={
                    DealerId:10018, //this.userinfo.DealerId,
                   	PoNum:this.detail.PO_NUM,//备货单号
                   	IsDelivery:this.IsDelivery,//是否送货
                   	IsTax:this.IsTax,//是否含税
                   	IsPacking:this.IsPacking,//是否带包装
                   	HasGoods: true//备货是否完成
                };
                this.loading=true;
                this.$store.dispatch({type : 'postData',url : api.AuditPopOrder,data:param,callBack :(data)=>{
                        _this.loading=false;
                        if(data.status=="success")
                        {
                        	Toast('操作成功返回上一级');
                        }
                    }
               })
		},
		AuditPopOrder2(){
                let _this=this;
                var param={
                    DealerId:10018, //this.userinfo.DealerId,
                   	PoNum:this.detail.PO_NUM,//备货单号
                   	IsDelivery:this.IsDelivery,//是否送货
                   	IsTax:this.IsTax,//是否含税
                   	IsPacking:this.IsPacking,//是否带包装
                   	HasGoods: false//备货是否完成
                };
                this.loading=true;
                this.$store.dispatch({type : 'postData',url : api.AuditPopOrder,data:param,callBack :(data)=>{
                        _this.loading=false;
                        if(data.status=="success")
                        {
                        	Toast('操作成功返回上一级');
                        }
                    }
               })
			}
		}
	}
</script>
