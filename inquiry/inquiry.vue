<!-- 询价单主体组件 -->
<template>
	<div id="inquiry_main" class="">
    	<!-- 顶部 -->
        <inquiry-header></inquiry-header>
		<div class="mian_content">
			<div v-for="(inquiry,index) in list" class="inquiry-list">
				<div class="mui-card-header xj_header">
					<label>询价单号：<em>{{inquiry.InqOrderCode}}</em></label>
					<span>{{inquiry.AddTime | dateformat('1') }}</span>
				</div>
				<div v-for="item in inquiry.Rows" class="mui-input-row mui-checkbox mui-left list_bd">
					<div class="Y_goods-lists-sub">
						<img :src="item.SmallPic">
						<div>
							<p>
								<a class="ovfh">{{item.ProdBrandName}} {{item.ProdName}}</a>
							</p>
							<p><span>{{item.Sku}}</span></p>
							<p>
								<span class="org">{{item.SalePrice | currency}}</span>
                               <span v-if="item.DealerType==0" class="mui-badge mui-badge-success span-color">品牌件</span>
							   <span v-if="item.DealerType==1&&item.ProdQuality" class="mui-badge mui-badge-success span-color">{{getProdQualityLabel(item.ProdQuality)}}</span>
							</p>
						</div>
					</div>
					<div class="Y_goods-lists-cd">
						<span><img src="../../assets/img/search/dxj_sincerity.png"/>{{formatDealerName(item.DealerName)}}</span>
						<span>产地：<em>{{item.Cus_OriginPlace}}</em></span>
					</div>
					<input name="checkbox"  v-model="item.checked" @click="itemcheck" type="checkbox" />
				</div>
				<div class="mui-card-footer inquiry_footer">
					<button type="button" class="mui-btn mui-btn-outlined" @click="deleteInquiry(inquiry,index)">删除询价单</button>
					<button type="button" class="mui-btn mui-btn-warning mui-btn-outlined" @click="tap()">立即下单</button>
				</div>
			</div>
			<div class="nothing" v-if="!loading&&!list.length">
				<img src="../../assets/img/inquiry/dxj_no.png" />
				<p>您暂时没有任何询价信息</p>
				<button type="button" class="mui-btn mui-btn-success" @click="gosearch">立即前往添加</button>

			</div>
		</div>
	</div>
</template>

<script>
	import inquiryHeader from 'components/inquiry/inquiry_header.vue'
    import {getProdQualityLabel,formatDealerName} from 'src/content/utils.js'
    import {SourcePlat} from 'src/content/status'
    import { mapActions, mapGetters } from 'vuex'
    import { Toast } from 'mint-ui';
    import api from 'src/api/urls.js'
    import { Trim } from 'src/content/utils.js';
	export default {
		name: 'inquiry',
        components:{
            inquiryHeader
        },
		data() {
			return {
                list:[],
                loading:false,
                wating:false,
                pagesize:5,
                pageindex:1,
                count:0,
                pagetotal:0
			}
		},
		computed: {
            ...mapGetters(
                {userinfo:'UserInfo'}
            )
		},
    	mounted(){
            this.$nextTick(()=>{
                this.query();
            })
	    },
		methods: {
            query(){
                let _this=this;
                var param={
                    Page:this.pageindex,
                    PageSize:this.pagesize,
                    DealerId:this.userinfo.DealerId,
                    DealerName:this.userinfo.DealerName,
                    PopUserName:this.userinfo.UserName,
                    SourcePlat:SourcePlat
                };
                this.loading=true;
                this.$store.dispatch({type : 'postData',url : api.GetInquiryOrderList,data:param,callBack :(data)=>{
                        _this.loading=false;
                        if(data.status=="success")
                        {
                            let result=JSON.parse(data.result);
                            _this.pagetotal=result.Pages;
                            _this.count=result.Count;
                            result.Rows.forEach(x=>{
                                x.Rows.forEach(k=>{
                                    k.state=0;
                                    if(k.DealerList.length)
                                    {
                                        k.state=k.DealerList[0].OfferStatus; //0:未报价 1:有效 2:过期
                                    }
                                    k.checked=false;
                                })

                            });
                            _this.list=result.Rows;
                            console.log(result);
                        }
                    }
                })
            },
			tap: function() {
				mui.toast('开发中...');
			},
			deleteInquiry: function(item,index) {
                let _this=this;
                let param={
                    InqOrderCode:item.InqOrderCode,
                    SourcePlat:SourcePlat,
                    DealerId:this.userinfo.DealerId,
                    DealerName:this.userinfo.DealerName,
                    UserName:this.userinfo.UserName,
                    PopUserName:this.userinfo.UserName,
                };
                console.log(param);
				mui.confirm('确认要删除已选中的配件？', "提醒", function(e) {
					if(e.index == 1) {
                        _this.$store.dispatch({type : 'postData',url : api.DeleteInquiryOrder,data:param,callBack :(data)=>{
                                if(data.status=="success")
                                {
                                    _this.list.splice(index,1);
                                    mui.toast('删除成功');
                                }else{
                                    mui.toast('删除失败,请稍后再试');
                                }
                            }
                        })
						
					} else {
						
					}
				})
			},
            itemcheck(item){
                item.checked=!item.checked;
                let res=this.list.every(x=>{
                    return x.checked==true;
                })
                if(res){
                    this.listchecked=true;
                }else{
                    this.listchecked=false;
                }
                
            },
            getProdQualityLabel(val){
                return getProdQualityLabel(val);
            },
            formatDealerName(val){
                return formatDealerName(val);
            },
            gosearch(){
                this.$router.push('/search');
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