<!-- 适用车型主体组件 -->
<template>
	<div id="useTo" class="" v-show="value">
        <mt-header :title="title" :fixed="is_fixed" >
			<mt-button @click.native="close" slot="left">
					<a class=" mui-icon mui-icon-left-nav mui-pull-left"></a>
			</mt-button>
		</mt-header>
		<div class="mian_content">
			<ul class="useTo-items">
				<li v-for="item in list" class="useTo-item-content">
					<p class="item-title">{{item.text}}</p>
					<p class="item-use">{{item.desc}}</p>
				</li>
			</ul>
		</div>
	</div>
</template>

<script>
import Vue from 'vue'
export default {
	name: 'useTo',
    props:{
        title:{
            type:String,
            default:""
        },
        list:{
            type:Array,
            default:function(){
                return []
            }
        },
        value:{
            type:Boolean,
            default:false
        }
    },
	data () {
		return {
			
		}
	},
	computed : {

	},
	methods : {
		close(){
            this.$emit('input',false);
        }
	}
}

</script>
<style>
	#useTo .useTo-items li{
		width: 100%;
		padding: 0.4rem;
		border-top: solid 1px #dfdfdf;
	}
	#useTo .useTo-items p{
		margin: 0;
	}
	#useTo .item-title{
		font-size: 0.75rem;
		line-height: 1rem;
	}
	#useTo .item-use{
		font-size: 0.65rem;
		line-height: 0.8rem;
		color: #e25d2e;
	}
</style>
