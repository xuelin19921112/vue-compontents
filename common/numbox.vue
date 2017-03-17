<!--数字输入框-->
<template>
	<div class="numbox fl text-center" >
		<button class="reduceBtn" type="button" :class="{'disabled':value.quantity<=minvalue}" @click="reduce">-</button>
		<input  class="numboxInput" type="number"  v-model="value.quantity "/>
		<button class="addBtn" type="button"  @click="add">+</button>
	</div>
</template>
<script>

export default {
    name: 'numbox',
    data() {
        return { 
        }
    },
	props:{
		value:{
			type:Object
		},
		minvalue:{
			type:Number,
			default:1
		}
	},
	computed:{
	},
	watch:{
	},
    methods: {
	  reduce(){
        if(this.value.quantity>this.minvalue){
			this.value.quantity--;
		}
	  },
	  add(){
		    this.value.quantity++;
	  }
    }
}

</script>
<style>
	.numbox{
		width: 50%;
		padding: 0.775rem 0;
		background: #fff;
		border-top: solid 1px #dfdfdf;
		font-size: 0.75rem;
		line-height: 1.15rem;
	}
	.numbox .reduceBtn,.numbox .addBtn{
		width: 1.15rem;
		height: 1.15rem;
		line-height:1.15rem;
		border-radius: 50%;
		background: #777;color: #fff;
		padding: 0;
	}
	.numbox .numboxInput{
		width: 2.5rem;
		height: 1.15rem;
		line-height:1.15rem;
		border: none;
		color: #000;
	}
	.disabled{
		background: #eee!important;
		border: solid 1px #eee;
	}
</style>