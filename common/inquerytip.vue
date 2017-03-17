<template>
    <div v-if="value" id="add-success">
      	<mt-header :title="title"  >
			<!--<mt-button  slot="left">
					<a class=" mui-icon mui-icon-left-nav mui-pull-left" @click="close"></a>
			</mt-button>-->
		</mt-header>
		<div class="success-content">
			<img src="../../assets/img/search/xcx_xj_success.png" />
			<p><slot></slot></p>
			<div class="group-btn">
				<span class="mui-btn mui-btn-success">
					<slot name="s1"></slot>
				</span>
				<span class="mui-btn mui-btn-danger">
					<slot name="s2"></slot>
				</span>
			</div>
		</div>
    </div>
</template>
<script>
export default {
    name: '',
    components: {
    },
    props:{
      value:{
         type:Boolean,
         default:false
      },
      title:{
         type:String,
         default:""
      }
     
    },
    data() {
        return { 

        }
    },
    methods: {
	  close(){
          this.$emit('input',false);
      }
    }
}

</script>

<style>
	#add-success {
		margin-bottom: 2rem;
	}
	#add-success .mint-header{
		background-color: #fff;
		color: #000;
	}
	#add-success .success-content{
		margin-top: 2rem;
		border-top: solid 1px #dfdfdf;
		padding-top: 1.75rem;
		text-align: center;
		font-size: 0.7rem;
		line-height: 1rem;
	}
	#add-success .success-content p{
		margin-bottom: 0;
		color: #666;
	}
	#add-success .success-content img{
		width: 5.25rem;
		height: 5.25rem;
	}
	#add-success .group-btn{
		margin-top: 2.5rem;
	}
	#add-success .mui-btn-success{
		background-color: #7bb951;
		border: solid 1px #7bb951;
		padding: 0.5rem 1.5rem;
		margin-right: 0.8rem;
	}
	#add-success .mui-btn-danger{
		background-color: #ea7544;
		border: solid 1px #ea7544;
		padding: 0.5rem 1.5rem;
	}
</style>