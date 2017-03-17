<!-- 注册成功组件 -->
<template>
	<div id="register-success" class="">
		<mt-header title="注册成功" :fixed="is_fixed" >
			<mt-button  slot="left">
					<a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
			</mt-button>
		</mt-header>
		<div class="success-content">
			<img src="../../assets/img/login/success.png" />
			<p>恭喜您，账号已经注册成功</p>
			<p>{{timedown}}s后自动跳转到首页</p>
			<div class="group-btn">
              <router-link to="/">
				<span class="mui-btn mui-btn-success">
					立即找配件
				</span>
             </router-link>
			</div>
		</div>
	</div>
</template>
<script>

export default {
	name: 'register-success',
	data () {
		return {
			is_fixed : true,
			timedown:3
		}
	},
	mounted(){
		let _this=this;
        _this.$nextTick(function () {
           _this.countDown(3);
		})
	},
	methods : {
        countDown(number){
		   let _this=this;
		   _this.timedown=number;
           var index=setInterval(function(){
				_this.timedown--;
				if(_this.timedown<=0)
				{
					clearInterval(index);
					_this.$router.push('/');
				}
			},1000);
		}
	}
}
</script>
<style>
	.font-color-green{
		color: #7bb951;
	}
	#register-success {
		margin-bottom: 2rem;
	}
	#register-success .mint-header{
		background-color: #fff;
		color: #000;
	}
	#register-success .success-content{
		margin-top: 2rem;
		border-top: solid 1px #dfdfdf;
		padding-top: 1.75rem;
		text-align: center;
		font-size: 0.7rem;
		line-height: 2rem;
	}
	#register-success .success-content p{
		margin-bottom: 0;
		color: #666;
	}
	#register-success .success-content img{
		width: 2.5rem;
		height: 2.5rem;
	}
	#register-success .group-btn{
		margin-top: 2.5rem;
	}
	#register-success .mui-btn-success{
		background-color: #7bb951;
		border: solid 1px #7bb951;
		padding: 0.5rem 5.5rem;
		margin-right: 0;
	}
</style>