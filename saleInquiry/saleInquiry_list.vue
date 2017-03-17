<template>
    <div>
        <div class="inquiry-content-items">
            <ul>
                <li v-for="item in list" class="item-content">
                    <template v-for="row in item.Rows">
                        <div class="title fl bdb">
                            <span class="buyer fl">买手：{{item.DealerName}}</span>
                            <span class="states fr font-color-red">{{GetState(item.State)}}</span>
                        </div>
                        <div class="content fl">
                            <ul  class="content-left fl bdb">
                                <li><span>LK132332452</span><span style="padding-left: 1.4rem;">{{row.ProdBrandName}}  {{row.ProdName}}</span></li>
                                <li><span>期望价：</span><span class="font-small font-color-red">￥</span><span class="font-color-red">{{row.MindPrice | currency}}</span><span
                                        class="font-color-grey"> ({{item.IsTax==1?'含税':'不含税'}}) </span></li>
                                <li><span>备注：</span><span style="padding-left: 0.2rem;">{{row.sellMemo}</span></li>
                            </ul>
                            <ul class="content-right">
                                <li><span v-if="row.ProdQuality" class="mui-badge mui-badge-success">{{row.ProdQuality}}</span></li>
                                <li><span>x{{row.Quantity}}</span></li>
                            </ul>
                        </div>
                        <div class="time fl">
                            <span class="nowtime fl font-color-grey">{{item.AddTime | dateformat('1') }}</span>
                            <router-link to="/quotation">
                                <button type="button" class="mui-btn mui-btn-danger offer fr">报价</button>
                            </router-link>
                        </div>
                     </template>
                </li>
            </ul>
        </div>
        <div >

        </div>
    </div>
</template>

<script>
import { ProdQuality } from '../../content/status.js';
	export default {
		components: {

		},
        props:{
          list:{
              type:Array,
              default:function(){
                  return [];
              }

          }
        },
		data() {
			return {
			   showAnswer:false,
               StateType:[{value:0,lable:"待报价"},{value:2,lable:"已报价"},{value:-1,lable:"已过期"}],

			}
		},
		computed: {

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
               
           }
		}
	}
</script>