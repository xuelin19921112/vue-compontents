<template>
        <div class="inquiry-content-items" id="saleInquiry-list">
            <ul v-infinite-scroll="loadMore" infinite-scroll-disabled=loading  infinite-scroll-immediate-check=true infinite-scroll-distance="10">
                <li v-for="row in list" class="item-content">
                    
                    <div class="title fl bdb">
                        <span class="buyer fl">买手：{{formatMoble(row.Mobile)}}</span>
                        <span class="states fr font-color-red" v-if='index==1'>待报价</span>
                    	<span class="states fr font-color-red" v-if='index==2'>已报价</span>
                    	<span class="states fr font-color-red" v-if='index==3'>已过期</span>
                    </div>
                    <div class="content fl">
                        <ul  class="content-left fl bdb">
                            <li><span>LK132332452</span><span style="padding-left: 1.4rem;">{{row.ProdBrandName}}  {{row.ProdName}}</span></li>
                            <li><span>期望价：</span><span class="font-small font-color-red">￥</span><span class="font-color-red">{{row.MindPrice | currency}}</span><span
                                    class="font-color-grey"> ({{row.IsTax==1?'含税':'不含税'}}) </span></li>
                            <li><span>备注：</span><span style="padding-left: 0.2rem;">{{row.sellMemo}</span></li>
                        </ul>
                        <ul class="content-right">
                            <li><span v-if="row.ProdQuality" class="mui-badge mui-badge-success">{{row.ProdQuality}}</span></li>
                            <li><span>x{{row.Quantity}}</span></li>
                        </ul>
                    </div>
                    <div class="time fl">
                        <span class="nowtime fl font-color-grey">{{row.AddTime | dateformat('1') }}</span>
                        <button type="button" class="mui-btn mui-btn-danger offer fr">报价</button>
                    </div>
                     
                </li>
            </ul>
        </div>

</template>

<script>
	import { ProdQuality } from '../../content/status.js';	
	import { InfiniteScroll } from 'mint-ui';
	import {SourcePlat} from 'src/content/status'
	import {mapActions,mapGetters} from 'vuex'
	import {Toast} from 'mint-ui';
	import api from 'src/api/urls.js'
	import {Trim,formatMoble} from '../../content/utils.js';
	export default {
		name:'saleInquiry-list',
//		components: {
//
//		},
        props:{
        	index: {
				type: Number,
				default: true
			},
          	list:{
               	type:Array,
				default: []

          	},
          	count:{
				type:Number,
				default:0
			}
        },
		data() {
			return {
			   loading:false,
			   showAnswer:false,
               StateType:[{value:0,lable:"待报价"},{value:2,lable:"已报价"},{value:-1,lable:"已过期"}]
			}
		},
		computed: {
			...mapGetters({
				userinfo: 'UserInfo'
			})
		},
        mounted(){
            this.$nextTick(()=>{
                this.GetProdQuality(1);
            })
        },
		methods: {
		   GetState(val)
           {
              var state= this.find(x=>{
                   return x.value==val;
               });
               return state ? state.lable:"";
           },
           GetProdQuality(val){
              
           },
           	formatMoble(val){
				return formatMoble(val);
			},
           loadMore() {
				console.log(this.loading);
				if(this.count<=0 || this.count==this.list.length)
				{
					return false;
				}
				this.$emit('loadnext',this.index);
				
			}
		}
	}
</script>